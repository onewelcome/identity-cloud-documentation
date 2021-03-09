# Securing resources

One of the most common use cases for using the Onegini Mobile Security Platform is to securely expose sensitive data to mobile apps via APIs. The Onegini SDK
uses the Bearer Token Usage RFC ([rfc6750](https://tools.ietf.org/html/rfc6750)) to talk to these protected API endpoints. This quick guide will show you how to
easily setup a resource gateway according to the specification to protect your APIs.

The resource gateway has the following responsibilities:

- [Extract the access token from an incoming request](#extract-the-access-token-from-an-incoming-request)
- [Introspect the access token at Onegini Access](#introspect-the-access-token-at-the-token-server)
- [Verify the access level through the introspection result](#verify-the-access-level-through-the-introspection-result)
- [Verify the Authentication Method through the introspection result](#verify-the-authentication-method-through-the-introspection-result)
- [Fetch and return the secured resource to the client](#fetch-and-return-the-secured-resource-to-the-client)
- [Handle errors according to RFC](#handle-errors-according-to-rfc)

[This example code](#the-code) shows how to implement these responsibilities in a simple Spring Boot application. For a production setup you might consider
using a third party product or a more scalable solution.

The example code exposes three endpoints. The `/resource/devices` endpoint, which returns a list of the registered user's devices from Onegini Access.
The `/resource/application-details` endpoint, which returns some general information about the client application coming from Onegini Access. And
the `/resource/user-id-decorated` endpoint, which returns the current user's user ID with some added affixes.

The rest of this document will follow a scenario where the client requests the devices endpoint. In this scenario the client first performs a GET request
to `/resources/devices` with an access token in the `Authorization` header (1). The resource gateway then introspects the access token with Onegini Access (2).
Onegini Access responds to this request with information about the access token (3), among which are the user ID (`sub`) and scopes associated with the token.
The `read` scope is required to access the devices endpoint. Onegini Access also provides us with an Authentication Method Reference (`amr`) field. This field
can be used to view how a user has authenticated. In the case of the device's endpoint we want to check this field to deny access to users who are implicitly
authenticated. If the token introspection result confirmed the required scope is present, the devices for the user assigned to the access token are fetched from
the secured API (4 and 5), in this scenario that is Onegini Access device API. Finally, the list of devices is returned to the client application (6).

```
  +-------------+                           +------------------+                                          +-----------------+                               
  | Onegini SDK | ---- (1) get devices ---> | Resource Gateway |  ---- (2) introspect access token --->   | Onegini Access  |                               
  | (Client)    |                           |                  |                                          |                 |                               
  |             | <--- (6) user devices --- |                  |  <---- (3) introspection response -----  |                 |                               
  +-------------+                           +------------------+                                          +-----------------+    
                                                    ^ |
                                                    | |                                                   +-----------------+ 
                                                    | +------------------ (4) get user devices ---------> | Resource Server | 
                                                    |                                                     | (Onegini Access)| 
                                                    +---------------------- (5) user devices ------------ |                 |
                                                                                                          +-----------------+ 
```

To configure a resource gateway in Onegini Access, you might want to consult
the [Resource gateway configuration](../../topics/general-app-config/resource-gateway/resource-gateway.md) topic guide.

## Extract the access token from an incoming request

The Onegini SDK provides the access token in the `Authorization` header with the `Bearer` prefix. The access token is included in plain text so *no*
encoding.

Example request:

```http
GET /resources/devices HTTP/1.1
Authorization: Bearer 5F09FC2DD7DCDB72ABF1A6C026DF8FFB9D7D1F4AD069E34F0A6EC6206A593420
Host: www.onegini.com:9999
Connection: close
```

The following example code extracts the access token from the HTTP Header:

```java

@Service
public class AccessTokenExtractor {

  public String extractFromHeader(final String authorizationHeaderValue) {
    if (isInvalidAuthorizationHeaderFormat(authorizationHeaderValue)) {
      final String message = String.format("Authorization header value `%s` does not contain an access token.", authorizationHeaderValue);
      throw new NoAccessTokenProvidedException(message);
    }

    return StringUtils.removeStart(authorizationHeaderValue, BEARER_PREFIX);
  }

  private boolean isInvalidAuthorizationHeaderFormat(final String authorizationHeader) {
    return StringUtils.isBlank(authorizationHeader)
        || !StringUtils.startsWithIgnoreCase(authorizationHeader, BEARER_PREFIX)
        || authorizationHeader.length() == BEARER_PREFIX.length();
  }

}
```

## Introspect the access token at Onegini Access

The received access token should be send to Onegini Access for introspection. For the API description see Onegini
Access [introspection API reference](../../api-reference/token-introspection.md).

The following code executes the introspection requests and checks if the the token is valid according to Onegini Access:

```java

@Service
public class TokenIntrospectionService {

  @Resource
  private TokenIntrospectionRequestExecutor tokenIntrospectionRequestExecutor;

  public TokenIntrospectionResult introspectAccessToken(final String accessToken) {
    final TokenIntrospectionResult tokenIntrospectionResult = tokenIntrospectionRequestExecutor.execute(accessToken).getBody();
    final boolean tokenInvalid = !tokenIntrospectionResult.isActive();

    if (tokenInvalid) {
      throw new InvalidAccessTokenException("Token introspection result: invalid token");
    }

    return tokenIntrospectionResult;
  }

}
```

The `TokenIntrospectionRequestExecutor` was used to execute the actual HTTP Request:

```java

@Service
public class TokenIntrospectionRequestExecutor {
  private static final String TOKEN = "token";
  private static final String ENDPOINT_INTROSPECT = "/api/v1/token/introspect";

  @Resource
  private TokenServerConfig tokenServerConfig;
  @Resource
  private RestTemplate restTemplate;

  public ResponseEntity<TokenIntrospectionResult> execute(final String accessToken) {
    final HttpEntity<?> entity = createRequestEntity(accessToken);
    final String url = tokenServerConfig.getBaseUri() + ENDPOINT_INTROSPECT;
    final ResponseEntity<TokenIntrospectionResult> response;

    try {
      response = restTemplate.exchange(url, HttpMethod.POST, entity, TokenIntrospectionResult.class);
    } catch (final RestClientException exception) {
      throw new TokenServerException(exception);
    }

    return response;
  }

  private HttpEntity<?> createRequestEntity(final String accessToken) {
    final HttpHeaders headers = createTokenIntrospectionRequestHeaders();
    final MultiValueMap<String, String> formData = new LinkedMultiValueMap<>();

    formData.add(TOKEN, accessToken);

    return new HttpEntity<Object>(formData, headers);
  }

  private HttpHeaders createTokenIntrospectionRequestHeaders() {
    final HttpHeaders headers = new HttpHeaders();
    final String authorizationHeaderValue = new BasicAuthenticationHeaderBuilder()
        .withUsername(tokenServerConfig.getClientId())
        .withPassword(tokenServerConfig.getClientSecret())
        .build();

    headers.add(AUTHORIZATION, authorizationHeaderValue);
    headers.add(CONTENT_TYPE, MediaType.APPLICATION_FORM_URLENCODED_VALUE);

    return headers;
  }

}
```

## Verify the access level by means of the introspection result

To check if the required access level is met, in other words if the end user gave consent for certain scopes to the application, the scopes need to be
validated. In this case the `read` scope is required. This is example also includes a method to validate if the `application-details` scope is granted.

```java

@Service
public class ScopeValidationService {

  public static final String SCOPE_READ = "read";
  public static final String SCOPE_APPLICATION_DETAILS = "application-details";

  public void validateReadScopeGranted(final String grantedScopes) {
    validateScopeGranted(grantedScopes, SCOPE_READ);
  }

  public void validateApplicationDetailsScopeGranted(final String grantedScopes) {
    validateScopeGranted(grantedScopes, SCOPE_APPLICATION_DETAILS);
  }

  private void validateScopeGranted(final String grantedScopes, final String scope) {
    if (StringUtils.isBlank(grantedScopes)) {
      throw new ScopeNotGrantedException("No scopes granted to access token");
    }

    final String[] scopes = StringUtils.split(grantedScopes, SPACE);
    final boolean scopeNotGranted = !ArrayUtils.contains(scopes, scope);
    if (scopeNotGranted) {
      final String message = String.format("Scope %s not granted to provided access token.", scope);
      throw new ScopeNotGrantedException(message);
    }
  }

}
```

## Verify the Authentication Method by means of the introspection result

The `TokenTypeValidationService` class verifies whether the user has authenticated using implicit authentication or not.

```java

@Service
public class TokenTypeValidationService {

  public void validateImplicitAuthenticationToken(final TokenType[] tokenTypes) {
    if (isNoImplicitAuthenticationToken(tokenTypes)) {
      throw new InvalidAccessTokenException("Token is not an implicit authentication access token");
    }
  }

  public void validateNoImplicitAuthenticationToken(final TokenType[] tokenTypes) {
    if (isImplicitAuthenticationToken(tokenTypes)) {
      throw new InvalidAccessTokenException("Token is an implicit authentication access token");
    }
  }

  private boolean isImplicitAuthenticationToken(final TokenType[] tokenTypes) {
    return ArrayUtils.contains(tokenTypes, IMPLICIT_AUTHENTICATION);
  }

  private boolean isNoImplicitAuthenticationToken(final TokenType[] tokenTypes) {
    return !isImplicitAuthenticationToken(tokenTypes);
  }
}
```

In this scenario, we do not want implicitly authenticated users to access the devices API. Therefore, the controller should check if the user is _not_
implicitly authenticated by passing the Authentication Method Reference Values (amr) from the introspection result:

```java
tokenTypeValidationService.validateNoImplicitAuthenticationToken(tokenIntrospectionResult.getAmr());
```

## Fetch and return the secured resource to the client

After all checks are passed successfully the devices for the user in the access token can be fetched and returned to the original caller. The
[device API](../../api-reference/end-user/device-v4.md#list-devices) can be reached via uri `/api/v4/users/{userId}/devices`.

```java

@Service
public class DeviceApiRequestService {

  public static final String DEVICE_API_PATH = "/api/v2/users/{user_id}/devices";

  @Resource
  private RestTemplate restTemplate;
  @Resource
  private DeviceApiConfig deviceApiConfig;

  public ResponseEntity<Devices> getDevices(final String userId) {
    final HttpEntity<?> requestEntity = createRequestEntity();
    final String uri = deviceApiConfig.getServerRoot() + DEVICE_API_PATH;

    return restTemplate.exchange(uri, HttpMethod.GET, requestEntity, Devices.class, userId);
  }

  private HttpEntity<?> createRequestEntity() {
    final HttpHeaders headers = new HttpHeaders();
    final String authorizationHeaderValue = new BasicAuthenticationHeaderBuilder()
        .withUsername(deviceApiConfig.getUsername())
        .withPassword(deviceApiConfig.getPassword())
        .build();
    headers.add(AUTHORIZATION, authorizationHeaderValue);

    return new HttpEntity<>(headers);
  }
}
``` 

Alternatively a resource can be requested that is not user specific. For example you have some content you only want to share via a mobile app and not being
publicly available on the web. For that an anonymous resource call can be performed. This call uses an access token which does not have a user assigned. An
application needs to have the `Anonymous resource calls` flow enabled in Onegini Access to be able to receive such an access token.

The example resource gateway exposes an endpoint `/resources/application-details` which returns the details of an application like the application version. This
information is fetched from the access token introspection result and can be used with a user and an anonymous access token. A prerequisite is that
the `application-details` scope is granted to the access token that is used.

## Handle errors according to RFC

The RFC defines specific errors for certain error scenarios, see Bearer Token Usage Error Codes
RFC ([rfc6750 par 3.1](https://tools.ietf.org/html/rfc6750#section-3.1)).

```java
public class ErrorResponseBuilder {

  public static ResponseEntity<ErrorResponse> buildBadRequestResponse() {
    final String error = "invalid_request";
    final String errorDescription = "The request is missing a required parameter";

    final ErrorResponse errorResponse = new ErrorResponse(error, errorDescription);
    return ResponseEntity.badRequest().body(errorResponse);
  }

  public static ResponseEntity<ErrorResponse> buildInvalidScopeResponse() {
    final String error = "insufficient_scope";
    final String errorDescription = "The request requires higher privileges than provided by the access token.";

    final ErrorResponse errorResponse = new ErrorResponse(error, errorDescription);
    return ResponseEntity.status(FORBIDDEN).body(errorResponse);
  }

  public static ResponseEntity<ErrorResponse> buildInvalidAccessTokenResponse() {
    final String error = "invalid_token";
    final String errorDescription = "The access token provided is expired, revoked, malformed, or invalid for other reasons.";

    final ErrorResponse errorResponse = new ErrorResponse(error, errorDescription);
    final String authenticateHeaderValue = BEARER_PREFIX + "error=\"" + error + "\", error_description=\"" + errorDescription + "\"";
    return ResponseEntity.status(UNAUTHORIZED).header(WWW_AUTHENTICATE, authenticateHeaderValue).body(errorResponse);
  }
}
``` 

## The code

The complete [example resource gateway](https://github.com/Onegini/example-resource-gateway) is available as a public repository on GitHub.
