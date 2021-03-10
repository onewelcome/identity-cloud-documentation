# ID Token

The ID Token is a [JSON Web Token (JWT)](https://tools.ietf.org/html/rfc7519) that contains user profile information (like the user's name, email, and so forth), represented in the form of claims. These claims are statements about the user, which can be trusted if the consumer of the token can verify its signature.

An ID Token is part of the OpenID Connect specification and is issued as a result of a valid authorization request with an `openid` scope. Please refer to the [OpenID Connect topic guide](../oidc/index.md) for more information.

## Example ID Token

In this section you can see an example of an ID Token. A JWT contains three sections: a header, a payload and a signature. Only the header and payload sections are displayed in the example below.

### Header

```json
{
  "kid": "f463bf2c-81a6-4979-82a5-aa5d032b6fe5",
  "alg": "RS256"
}
```

### Payload

```json
{
  "at_hash": "VoF7cDD8ojOOHC_0qpYrAw",
  "sub": "1c0e2c84-b05f-4c23-9175-c238f70901be",
  "email_verified": true,
  "gender": "male",
  "iss": "https://token-server.onegini.com/oauth",
  "preferred_username": "John Doe",
  "given_name": "John",
  "locale": "en",
  "aud": [
    "client_id",
    "https://resource-server.example.com"
  ],
  "name": "John Doe",
  "nickname": "Johnny",
  "exp": 1537362840,
  "iat": 1537359240,
  "family_name": "Doe",
  "email": "john.doe@example.com"
}
```

The payload contains a number of claims. A claim is a piece of information that is asserted about an entity. An entity in this context can not only be a person but also other things like servers or things. An ID Token can contain different types of claims:

- Claims that tell something about the origin, authenticity and validity of the ID Token
- Claims that tell something about the user's identity for which this ID Token was created.

The ID Token contains different user related claims depending on the scopes that the application (Relying Party) requested during the authorization request. Please see the [OpenID Connect topic guide](../oidc/overview/overview.md) for information about the possible claims that an ID Token can contain.

Below you can find a description for the generic claims that are in the ID Token regardless of the scopes that have been requested.

| Claim   | Description
|---------|------------
| at_hash | Hash of the access token that was issued. More information can be found on the [Open ID Core specification](https://openid.net/specs/openid-connect-core-1_0.html#CodeIDToken)
| iss     | The issuer of this ID Token. This will always be Onegini Access
| aud     | The audience for which this ID Token is intended. The Relying Party client ID that requested this ID Token is always included as the audience.
| sub     | The user identifier for then end-user that this ID Token is representing
| urn:onegini.com:oidc:group_policies | User's policies and group memberships. Requires [configuration of the Delegated Administration](../../topics/dum-report/index.md).
