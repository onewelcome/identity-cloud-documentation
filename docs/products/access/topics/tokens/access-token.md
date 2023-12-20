# Access Token

An Access Token is a short-lived credential that can be used by an application to access an API. Its purpose is to inform that the bearer of this token has 
been authorized to access a specific API. Access Tokens should be sent to an API according to the 
[Bearer token Usage specification](https://tools.ietf.org/html/rfc6750). Specifically, the Access Token should be sent to the API in the HTTP `Authorization` 
header.

Refer to the API reference how to [obtain an access token](../../api-reference/description-oauth-endpoint.md).

## Opaque token

The Opaque access token is a random, 32-bits hex-encoded, string (64 characters). It does not contain any information about the validity of the token. The 
Opaque token is returned to all clients of a [Mobile app](../mobile-apps/index.md) and to the [Web clients](../web-clients/index.md) for which the Opaque token 
is configured.

### Example Opaque Access Token

```
   E19C77561880BBF24F9E60B0D9051401FE2216A93F8683438A0DF2169CFE078F
``` 

## JSON Web Token (JWT)
OneWelcome Access can issue a [JWT](https://tools.ietf.org/html/rfc7519) as access token. However, the receiver does not have to treat it as a JWT but can also 
treat it as an opaque token and present it to OneWelcome Access for validation. Refer to the 
[Token Introspection documentation](../../api-reference/token-introspection.md) for details on validation an access token.

The JWT token is returned when this is [configured for a web client](../web-clients/web-client-configuration.md). It contains the user identifier when the 
access token is created for a specific user. 

### Example JWT Access Token

In this section you can see an example of a JWT Access Token. A JWT contains three sections: a header, a payload and a signature. Only the header and payload 
sections are displayed in the example below.

#### Header

```json
{
  "kid": "f463bf2c-81a6-4979-82a5-aa5d032b6fe5",
  "alg": "RS256"
}
```

#### Payload

```json
{
  "ver": 1,
  "jti": "AT.d405c8b0-2afc-4720-a567-e890fecd28b2",
  "iss": "https://token-server.onegini.com/oauth",
  "aud": "profile-api",
  "iat": 1537437991,
  "nbf": 1537437991,
  "exp": 1537441591,
  "cid": "example-client",
  "scp": [
    "profile",
    "read"
  ],
  "scope" : "profile read",
  "sub": "1c0e2c84-b05f-4c23-9175-c238f70901be",
  "usl": 5
}
```

The payload of a JWT Access Token contains a number of claims. These claims can be used to validate the Access Token but also tell for whom and what 
authorizations have been granted.

| Claim   | Description
|---------|------------
| ver     | Version indication for this Access Token
| jti     | JWT ID. A unique identifier of this JWT
| iss     | Issuer of this Access Token
| aud     | Audiences that this token is intended for
| iat     | Time the Access Token was issued
| nbf     | Time before which the Access Token is not valid 
| exp     | Time the Access Token expires
| cid     | Client ID of the client that requested the Access Token
| scope   | String value containing a space-separated list of [scopes](../general-app-config/scopes/scopes.md) that were granted for this Access Token.
| scp     | [Deprecated] Array of scopes that were granted for this Access Token.
| sub     | User Identifier
| usl     | Usage Limit. Integer value that represents the usage limit for this Access Token 
| <span id="group-permissions">group_permissions</span> | [DEPRECATED] Stringified representation of the user's group memberships and permissions. Requires [configuration of Delegated Administration](../dum-report/index.md).<br />The claim `group_permissions` is omitted when the size of the JWT Access Token exceeds the limit. This is to prevent that the JWT Access Token cannot be used to request data. When a `group_permissions` is expected, but it exceeds the limit, it can be requested via the [token introspection](../../api-reference/token-introspection.md) endpoint. This claim will only be returned when the OAuth Client requesting the Access Token will have `Group permissions version` set to 'Legacy: V1' within its client configuration. 
| <span id="urn:onegini.com:oidc:group_policies">urn:onegini.com:oidc:group_policies</span> | User's policies and group memberships. Requires [configuration of the Delegated Administration](../dum-report/index.md).<br />The claim `urn:onegini.com:oidc:group_policies` is omitted when the size of the JWT Access Token exceeds the limit. This is to prevent that the JWT Access Token cannot be used to request data. If the length of the token is exceed, the `urn:onegini.com:oidc:group_policies` claim can be requested via the [token introspection](../../api-reference/token-introspection.md) or [user-info](../../api-reference/oidc/user-info.md) endpoints or included in the [id-token](./id-token.md). 
