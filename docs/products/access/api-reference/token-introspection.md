# Token introspection

This section describes the Token Introspection API provided by the Access. Token introspection can be used to validate access tokens.

## API description

This endpoint implements token introspection according to [RFC 7662 - OAuth 2.0 Token Introspection](https://tools.ietf.org/html/rfc7662).

Endpoint:
```http
POST /oauth/api/v2/token/introspect
```

This endpoint requires authentication with [valid API client credentials](../topics/technical-app-management/api-configuration/api-configuration.md). It accepts the following request parameter(s):

| Parameter            | Required | Example value | Description
|----------------------|----------|---------------|---------------------------------------------------------------------------------------------------------
| token                | yes      | 24CAD1DA628B360B7EF85C30E423D0AB0FC0DCF8C7EB8CAD9640DBABE43910F9 | The value of the access token
| refresh_user_details | no       |  true         | The properties of the `user_details` object from the [user info endpoint](../topics/general-app-config/identity-providers/identity-providers.md#configure-user-info-endpoint) are cached after the first request. Use this parameter to force fetching the user details again from the user info endpoint. Performance may decrease when you add this parameter to every request.  


The introspection response object described in [paragraph 2.2](https://tools.ietf.org/html/rfc7662#section-2.2) of the token introspection specification is extended in our implementation with a number of custom parameters:


| Attribute                           | Description
|-------------------------------------|------------
| usage_count                         | When a usage limit is set, this field indicates the number of times the access token has already been used.
| usage_limit                         | When a usage limit is set, this field indicates the max number of times the access token can be used.
| user_details                        | List of details about the user. Depending on the configured [user detail mappings](../topics/general-app-config/identity-providers/identity-providers.md#attribute-mapping) in the identity provider this array can differ in size.                           |
| amr                                 | Authentication Methods References. A JSON array of strings that are identify the authentication method that was used during authentication. The possible values will be OneWelcome Access token types for now: `DEFAULT`, `FINGER_PRINT`, `IMPLICIT`, `CUSTOM_AUTHENTICATOR`.
| app_identifier                      | The identifier of the [mobile app](../topics/mobile-apps/index.md). Empty when the client is not an instance of a mobile app.
| app_platform                        | The platform of the mobile app. Empty when the client is not an instance of a mobile app.
| app_version                         | The [version of the mobile app](../topics/mobile-apps/app-configuration/app-version-management.md). Empty when the client is not an instance of a mobile app.
| app_identifier                      | The identifier of the [mobile app](../topics/mobile-apps/index.md). Empty when the client is not an instance of a mobile app.
| app_platform                        | The platform of the mobile app. Empty when the client is not an instance of a mobile app.
| app_version                         | The [version of the mobile app](../topics/mobile-apps/app-configuration/app-version-management.md). Empty when the client is not an instance of a mobile app.
| group_permissions                   | [DEPRECATED] Stringified representation of the user's group memberships and permissions. Requires [configuration of the DABP API](../topics/dum-report/index.md) and setting the [legacy group permissions](../topics/web-clients/web-client-configuration.md).
| urn:onegini.com:oidc:group_policies | User's policies and group memberships. Requires [configuration of the DABP API](../topics/dum-report/index.md).

## Example request

Example token introspection request:
```http
POST /oauth/api/v2/token/introspect HTTP/1.1
Host: onegini.example.com
Accept: application/json
Content-Type: application/x-www-form-urlencoded
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW

token=24CAD1DA628B360B7EF85C30E423D0AB0FC0DCF8C7EB8CAD9640DBABE43910F9
```

Example token introspection response for a _valid_ access token:
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "active": true,
  "scope": "read",
  "client_id": "D4CFB02DA92C083934665000199A09DE793C97F94C9714DE3D38C3E5D2985494",
  "token_type": "bearer",
  "exp": 1686258829,
  "iat": 1504011352,
  "nbf": 1504011352,
  "sub": "TestClientUserId",
  "app_identifier": "TestAppId",
  "app_version": "1.0",
  "app_platform": "ios",
  "usage_limit": 0,
  "user_details": {
    "firstName": "John",
    "lastName": "Doe",
    "phoneNumber": "0031612345678",
    "authenticationLevel": "1",
    "email": "john.doe@example.com"
  },
  "amr": [
    "DEFAULT",
    "FINGER_PRINT",
    "CUSTOM_AUTHENTICATOR",
    "IMPLICIT_AUTHENTICATION"
  ]
}
```

Example token introspection response for an _invalid_ access token:
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "active": false
}
```
