# OAuth endpoints

This section explains the OAuth API endpoints within OneWelcome Access. It's divided into the following subsections:
<!-- toc -->

For details about the OAuth endpoints we refer to the [OAuth specification](https://tools.ietf.org/html/rfc6749).

## Introduction

### Roles

OAuth defines four roles:

* Resource owner: the user
* Authorization server: OneWelcome Access. It's responsible for managing [tokens](../topics/tokens/index.md).
* Client: a website or app that wants to access the user's resources. Resources can be anything: personal data, documents, transactions, signatures, images, etc.
* Resource server: a server that contains the user's resources that are being accessed by the client. It grants access to the resource when the client provides 
a valid token. It is also referred to as Resource Gateway.

### Flows

The OAuth 2.0 specification describes the following flows to obtain tokens:

* [Authorization code](https://tools.ietf.org/html/rfc6749#section-4.1)
* [Client credentials](https://tools.ietf.org/html/rfc6749#section-4.4)

OneWelcome Access supports all of these flows. However, only the authorization code grant and client credentials grant are recommended.

When to use which flow: 

* The authorization code flow returns an access token that gives access to resources of a specific user. For example, when the user wants to check their 
insurance policy, the resource server needs to know on behalf of which user the client wants to access the policy. User interaction is often required during 
this flow.
* The client credentials flow returns an access token that gives access to resources for a specific client. For example, when the user requests the general 
terms and conditions document, the resource server needs to know which client requests access to this document. The resource server does not need to know on
behalf of which user the client requests access. Obtaining the access token can be automated and does not require user interaction.

## Authorization endpoint

The authorization endpoint is used in the authorization code flow. In this flow, the 
OneWelcome Access sends an access grant to the client. The client can exchange this access grant for an access token by calling the [token endpoint](#token-endpoint).

Endpoints: `GET /oauth/authorize` or `GET /oauth/v1/authorize`

Example request for the authorization code flow:

```http
GET /oauth/v1/authorize?response_type=code&client_id=exampleApp&state=xyz&redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fredirect HTTP/1.1
Host: onewelcome.example.com
```

Example success response for the authorization code flow:

```http
HTTP/1.1 302 Found
Location: https://client.example.com/redirect?code=myAccessGrant12345&state=xyz
```

### Specification

See the [OAuth specification section 3.1](https://tools.ietf.org/html/rfc6749#section-3.1) and the specification for [Proof Key for Code Exchange by OAuth
 Public Clients](https://tools.ietf.org/html/rfc7636) (PKCE).

### Authorization extensions

This endpoint allows additional request parameters. None of them are required. This section describes the parameters that are specific for the 
OneWelcome Access.

#### idp

Specifies the [identity provider](../topics/general-app-config/identity-providers/identity-providers.md) for the authentication. The value is the identifier of
the identity provider in OneWelcome Access.

Example value: `idp=idpId`

Example use case: an application that supports registration via an identity provider for future customers (prospects) and another identity provider for
existing customers. Both identity providers would be configured for this application.

#### external_idp

Preselects the external identity provider in OneWelcome CIM for authentication. The value starts with `urn:com:onegini:saml:idp:` followed by the
[identifier of the identity provider](https://docs-single-tenant.onegini.com/cim/stable/idp/administration/identity-providers.html) in OneWelcome CIM.
This feature requires [OneWelcome CIM to be used as an identity provider](../topics/general-app-config/identity-providers/identity-providers.md#configure-a-onewelcome-cim-identity-provider).

Example value: `external_idp=urn%3Acom%3Aonegini%3Asaml%3Aidp%3Adigid`

Example use case: an application that authenticates the user with the third party identity provider via OneWelcome CIM but wants to skip the login
screen of the latter.

#### external_idp_custom_param.*

This parameter can be used to populate a key value map in the [OneWelcome Extension](https://docs-single-tenant.onegini.com/cim/stable/idp/authentication/saml/saml-extension-custom-parameters.html)
as part of the SAML Authentication Request. The key value map can be used in the OneWelcome CIM product for customizing styling or logic in combination with the
[Session API](https://docs-single-tenant.onegini.com/cim/stable/idp/api-reference/session-api.html). This feature requires
 [OneWelcome CIM to be used as an identity provider](../topics/general-app-config/identity-providers/identity-providers.md#configure-a-onewelcome-cim-identity-provider).

Example value: `external_idp_custom_param.key1=val1&external_idp_custom_param.key2=val2&external_idp_custom_param.key2=val3`

This example shows the ability to pass multiple keys: `key1` and `key2`, and multiple values for a single key: `key2` has values `val2` and `val3`.

Example use cases are described in the [extension custom parameters topic guide](https://docs-single-tenant.onegini.com/cim/stable/idp/topic-guides/extension-custom-parameters.html).

#### app_view

Indication for OneWelcome CIM that the pages are shown within a mobile app. Passing this parameter is only needed when a
[web client](../topics/web-clients/index.md) is used for a mobile device (not recommended). The default value for [mobile apps](../topics/mobile-apps/index.md)
is `mobile`.

Example value: `app_view=mobile`

Example use cases:
* a login page that should not show the mobile login button when the user visits the login page from within the
mobile app.
* Pages shown in the mobile app should not get a navigation menu.

#### language

Specifies the locale for [translations](../topics/look-and-feel/translations/translations.md) during the authorization flow.

Example values: `en` or `en_US`

The contents of the `language` parameter are parsed as [Java Locale](http://www.oracle.com/us/technologies/java/locale-140624.html).
Supported formats for the `language` parameter:

* `languageCode`, example: `en`
* `languageCode_countryCode`, example: `en_US`
* `languageCode_countryCode_variant`, example: `en_US_east`

Example use case: the registration flow for the application should present all screens in the same language.

#### hook_context_custom_param.*

This parameter can be used to populate a `contextCustomParams` key value map in the [Access Token Web Hook](../topics/integration-extension/hooks/customize-access-token/customize-access-token-hook.md) and [User Details Web Hook](../topics/integration-extension/hooks/customize-user-details/customize-user-details-hook.md).

Example value: `hook_context_custom_param.key1=val1&hook_context_custom_param.key2=val2&hook_context_custom_param.key2=val3`

This example shows the ability to pass multiple keys: `key1` and `key2`, and multiple values for a single key: `key2` has values `val2` and `val3`. The `contextCustomParams` key value map, for above example value, will have entries:

| Key   | Value             |
|-------|-------------------|
| key1  | ["val1"]          |
| key2  | ["val2", "val3"]  |

Example use-case: providing context like 'on_behalf_of', 'on_behalf_of_type' to make integrating with a `Externalized Authorization` product possible.

### Authorization error extensions

Apart from the standard error parameters provided by the OAuth specification, in some error scenarios additional parameters are returned. These parameters are
meant to give more information about the specific error that happened.

#### Error cause parameters

OneWelcome Access uses external Identity Providers to authenticate users. In some scenarios authentication can fail, but it can be a bit unclear why it failed.

In case the SAML IdP is used and authentication failed due to a malformed request or due to some user interaction, the generic SAML error will be available in `onegini_saml_error`. To provide additional information the `onegini_error_cause` parameter is sometimes added in case of a specific error during the authorization flow.

The `onegini_saml_error` and `onegini_error_cause` parameters are included on top of the standard error response as defined by the OAuth specification [RFC6749, section 4.1.2.1](https://tools.ietf.org/html/rfc6749#section-4.1.2.1).
Currently, these fields are only provided when a SAML IdP is used for authentication by the end user.
The value of the `onegini_saml_error` attribute contains the SAML response status (e.g. `urn:oasis:names:tc:SAML:2.0:status:NoPassive`)
of the SAML response that was sent by the external IdP to the OneWelcome Access. For a list of SAML status codes see the [SAML 2.0 core specification, section 3.2.2.2](https://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)
The value of the `onegini_error_cause` can contain more detailed information about the error cause like custom error codes, which are especially useful in processes like on-boarding. Values can be, for example, `no_existing_customer`, `invalid_birthdate`, `fraud_detected` or whatever is defined as custom error in the user journey.

## Token endpoint

The token endpoint returns an access token for the following flows: 

* authorization code
* client credentials

Endpoints: `POST /oauth/token` or `POST /oauth/v1/token`

Example request for the authorization code flow:

```http
POST /oauth/v1/token HTTP/1.1
Host: server.example.com
Authorization: Basic ZXhhbXBsZUFwcDp0aGVTZWNyZXRUaGF0QmVsb25nc1RvVGhlRXhhbXBsZUFwcA==
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&code=myAccessGrant12345&redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fredirect
```

Example success response:

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache

{
  "token_type": "bearer",
  "access_token": "myAccessToken12345",
  "refresh_token": "myRefreshToken12345",
  "expires_in": 900,
  "profile_id": "static"
}
```

### Specification

See the [OAuth specification section 3.2](https://tools.ietf.org/html/rfc6749#section-3.2) and the specification for [Proof Key for Code Exchange by OAuth
 Public Clients](https://tools.ietf.org/html/rfc7636) (PKCE).

### Client Authentication

The token endpoint supports two types of client authentication:

* [Client Secret](https://tools.ietf.org/html/rfc6749#section-2.3.1)
* [Private Key JWT](https://tools.ietf.org/html/rfc7523)

For more information, see [Authentication Methods documentation](../topics/authentication-methods/authentication-methods.md).

## Revoke token endpoint

Use this endpoint to revoke tokens. The OneWelcome Access will immediately make them invalid, but clients or resource servers will not receive a 
notification. When the resource server requests token introspection for the token, the response does indicate that this token is no longer valid.

Endpoint: `POST /oauth/revoke` or `POST /oauth/v1/revoke`

Example request:

```http
POST /oauth/v1/revoke HTTP/1.1
Host: server.example.com
Authorization: Basic ZXhhbXBsZUFwcDp0aGVTZWNyZXRUaGF0QmVsb25nc1RvVGhlRXhhbXBsZUFwcA==
Content-Type: application/x-www-form-urlencoded

token=myAccessToken12345&token_type_hint=access_token
```

Example success response:

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
```

### Specification

See the OAuth specification for [token revocation](https://tools.ietf.org/html/rfc7009) and the specification for [Proof Key for Code Exchange by OAuth
 Public Clients](https://tools.ietf.org/html/rfc7636) (PKCE).

Note for refreshing and revoking the access token: When a public client uses the authorization code grant type with PKCE, the request parameter `client_id` is mandatory.
For *confidential* clients both the client id and the client secret are sent as basic authentication credentials via the `authorization` header.

## OAuth Device Flow Extension

### Introduction

The OAuth Device Flow is an extension to the standard OAuth 2.0 protocol, specifically designed for devices with no browser or limited input
capability. This flow enables users to authorize such devices in a user-friendly and secure manner. After entering the correct code, the
user proceeds through the standard authorization process, including authentication and, if enabled on the client, consent. For detailed
specifications, refer to [RFC 8628](https://tools.ietf.org/html/rfc8628).

### Authorization Process

The client initiates the authorization process by sending a POST request to the `/oauth/device_authorization` endpoint. This request
includes the client's unique identifier (`client_id`) and optionally the requested `scope` and identity provider (`idp`). The server
responds with a `device_code`, `user_code`, and `verification_uri`. The user must visit this URI in a browser and enter the user code to
authorize the device, after which they complete the standard authorization flow.

#### Request and Response Example:

- **Request**:
  ```http
  POST /oauth/device_authorization HTTP/1.1
  Host: example.com
  Content-Type: application/x-www-form-urlencoded

  client_id=your-client-id&scope=openid&idp=idp123
  ```

- **Response**:
  ```http
  HTTP/1.1 200 OK
  Content-Type: application/json

  {
    "device_code": "OVvqygRcDyw1zLda8Vh78OmVijaDSAPj09ebUIDZu9wP2tqsJgOW9x0ggFqNji4L",
    "user_code": "GMVRZTXC",
    "verification_uri": "https://example.com/oauth/device_authorization/verification",
    "verification_uri_complete": "https://example.com/oauth/device_authorization/verification?user_code=GMVRZTXC"
  }
  ```

### Token Generation

The client polls the `/oauth/token` endpoint for an access token using the `device_code` and its `client_id`. It's important to note that
there is no mechanism in place to limit the polling of this endpoint, and the `slow_down` error is not used. However, it is recommended to
implement a polling interval of at least 5 seconds, especially since this is also a requirement of the RFC in cases where the interval has
not been returned in the authorization request.

The server will respond with a pending authorization message or, upon successful authorization, the access token and related information.

#### Token Generation Examples

##### Unauthorized Client Example

- *Request*:
  ```http
  POST /oauth/token HTTP/1.1
  Host: example.com
  Content-Type: application/x-www-form-urlencoded

  device_code=OVvqygRcDyw1zLda8Vh78OmVijaDSAPj09ebUIDZu9wP2tqsJgOW9x0ggFqNji4L&client_id=client123&grant_type=urn:ietf:params:oauth:grant-type:device_code
  ```
- *Response*:
  ```http
  HTTP/1.1 400 Bad Request
  Content-Type: application/json

  {
    "error": "authorization_pending",
    "error_description": "The authorization request is still pending"
  }
  ```

Note: While the `400` status code typically indicates an error, in this context, it is an expected outcome signaling that the client's
authorization is still pending.

##### Authorized Client Example

- *Request*:
  ```http
  POST /oauth/token HTTP/1.1
  Host: example.com
  Content-Type: application/x-www-form-urlencoded

  device_code=OVvqygRcDyw1zLda8Vh78OmVijaDSAPj09ebUIDZu9wP2tqsJgOW9x0ggFqNji4L&client_id=client123&grant_type=urn:ietf:params:oauth:grant-type:device_code
  ```
- *Response*:
  ```http
  HTTP/1.1 200 OK
  Content-Type: application/json

  {
    "token_type": "bearer",
    "access_token": "D24A8C3742C613F547399C65BC9EC6462CD68F4213F3F085483DF895AFF40EE7",
    "refresh_token": "F41F194EFCDF64DA35221C756E281C40929667C96862709AF2F904F6A67B7C75",
    "expires_in": 900,
    "profile_id": "static"
  }
  ```

#### Error Handling

| Error           | Description                                                                           |
|-----------------|---------------------------------------------------------------------------------------|
| invalid_request | Missing a required parameter such as 'client_id' or 'device_code'.                    |
| invalid_grant   | Provided grant is invalid or doesn't match, or the device code has already been used. |
| expired_token   | The device code has expired before the token exchange or before authorization.        |
