# Stateless Custom Registration

OneWelcome Access has support for custom user registration without creating a profile. The Access Token created in the process has
the `Stateless` type and is not stored in the user's device. This makes it possible to implement support for App2App authentication
towards external identity schemas that do not allow you to make derived identities (like DigiD) in a custom registration script.
This means the app will be registered "just in time" each time a user selects the custom registration method.

## Enable stateless authentication

Stateless authentication can be enabled for mobile clients in the Access Admin.

To enable the feature, head to `Configuration` &rightarrow; `Applications` in Access Admin. Once there, you can create a new
application or edit an existing one. Either way, to enable stateless authentication, under `User authentication`, enable `Stateless authentication`.

## Send stateless custom registration request

Stateless authentication requires a configured [Custom API Identity Provider](../general-app-config/identity-providers/identity-providers.md#configure-a-custom-api-identity-provider).

To initiate stateless custom registration, the Complete Step request should include the `grant_type` parameter with the
value `urn:onewelcome:oauth2:grant_type:stateless_authentication`.

The feature can be used with both `ONE_STEP` and `TWO_STEP` flows. In both cases, the `grant_type` parameter must be provided in the Complete Step request.

Example request:

```http
POST /oauth/custom-registration/example-custom-registration-idp/complete HTTP/1.1
Host: onewelcome.example.com
Content-Type: application/json
Authentication: Basic Y2xpZW50OnNlY3JldA==

{
  "scope": [
      "openid","profile"
  ],
  "profile_id": "123EXI",
  "grant_type": "urn:onewelcome:oauth2:grant_type:stateless_authentication"
}
```

The returned custom registration response will contain a stateless Access Token.

The returned token can be validated using [token introspection](../../api-reference/token-introspection.md).
The introspection result will contain `STATELESS_AUTHENTICATION` in the AMR claim.

Example token introspection response for a stateless Access Token:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "active": true,
  "scope": "openid profile",
  "client_id": "D982FFA7D6BA78654ED77E67CEF02F26CEF2155DD451CB071CC134080D27F58C",
  "token_type": "Bearer",
  "exp": 899986,
  "iat": 1693221054,
  "nbf": 1693221054,
  "sub": "TestClientUserId",
  "app_identifier": "TestAppId",
  "app_version": "1.0",
  "app_platform": "ios",
  "usage_limit": 0,
  "user_details": {
    "firstName": "John",
    "lastName": "Doe"
  },
  "amr": [
    "STATELESS_AUTHENTICATION"
  ]
}
```

## Stateless Access Token limitations

- [Mobile authentication](../mobile-apps/mobile-authentication/mobile-authentication.md) enrollment is not possible when a Stateless Access
  Token is provided in the request.
- [Implicit authentication](../mobile-apps/implicit-authentication/implicit-authentication.md) is not possible for a user that only has a
  Stateless Access Token.
