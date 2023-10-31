# Discovery API

This API is the implementation of [OpenID Connect Discovery 1.0](https://openid.net/specs/openid-connect-discovery-1_0.html) specification.
It is used to publish OIDC-related metadata that can be used by Relying Party to understand which features are available and how to interact with it.

Endpoint: `GET /oauth/.well-known/openid-configuration`

**Example response**

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
 
{
    "issuer": "https://tokenserver.example.com",
    "authorization_endpoint": "https://tokenserver.example.com/oauth/v1/authorize",
    "token_endpoint": "https://tokenserver.example.com/oauth/v1/token",
    "jwks_uri": "https://tokenserver.example.com/oauth/v1/keys",
    "userinfo_endpoint": "https://tokenserver.example.com/oauth/v1/userinfo",
    "revocation_endpoint": "https://tokenserver.example.com/oauth/v1/revoke",
    "response_types_supported": [
        "code"
    ],
    "subject_types_supported": [
        "public"
    ],
    "id_token_signing_alg_values_supported": [
        "rs256",
        "rs384",
        "rs512",
        "ps256",
        "ps384",
        "ps512",
        "es256",
        "es384",
        "es512"
    ],
    "token_endpoint_auth_methods_supported": [
        "client_secret_basic",
        "client_secret_post",
        "private_key_jwt"
    ],
    "scopes_supported": [
        "address",
        "phone",
        "openid",
        "profile",
        "email"
    ],
    "claims_supported": [
        "sub",
        "name",
        "given_name",
        "family_name",
        "middle_name",
        "nickname",
        "preferred_username",
        "profile",
        "picture",
        "website",
        "email",
        "email_verified",
        "gender",
        "birthdate",
        "zoneinfo",
        "locale",
        "phone_number",
        "phone_number_verified",
        "address",
        "updated_at",
        "custom_attributes"
    ],
    "check_session_iframe": "https://tokenserver.example.com/oauth/v1/checksession",
    "end_session_endpoint": "https://tokenserver.example.com/oauth/v1/logout",
    "frontchannel_logout_supported": true,
    "frontchannel_logout_session_supported": false,
    "id_token_encryption_alg_values_supported": [
        "RSA-OAEP-256",
        "RSA-OAEP-384",
        "RSA-OAEP-512",
        "ECDH-ES",
        "ECDH-ES+A128KW",
        "ECDH-ES+A192KW",
        "ECDH-ES+A256KW"
    ],
    "id_token_encryption_enc_values_supported": [
        "A256GCM",
        "A192GCM",
        "A128GCM",
        "A128CBC-HS256",
        "A192CBC-HS384",
        "A256CBC-HS512"
    ],
    "acr_values_supported": [
        "urn:onegini.com:oidc:authentication_level:1",
        "urn:onegini.com:oidc:authentication_level:1:identification_level:1",
        "urn:onegini.com:oidc:authentication_level:1:identification_level:2",
        "urn:onegini.com:oidc:authentication_level:1:identification_level:3",
        "urn:onegini.com:oidc:authentication_level:1:identification_level:4",
        "urn:onegini.com:oidc:authentication_level:2",
        "urn:onegini.com:oidc:authentication_level:2:identification_level:1",
        "urn:onegini.com:oidc:authentication_level:2:identification_level:2",
        "urn:onegini.com:oidc:authentication_level:2:identification_level:3",
        "urn:onegini.com:oidc:authentication_level:2:identification_level:4",
        "urn:onegini.com:oidc:authentication_level:3",
        "urn:onegini.com:oidc:authentication_level:3:identification_level:1",
        "urn:onegini.com:oidc:authentication_level:3:identification_level:2",
        "urn:onegini.com:oidc:authentication_level:3:identification_level:3",
        "urn:onegini.com:oidc:authentication_level:3:identification_level:4",
        "urn:onegini.com:oidc:authentication_level:4",
        "urn:onegini.com:oidc:authentication_level:4:identification_level:1",
        "urn:onegini.com:oidc:authentication_level:4:identification_level:2",
        "urn:onegini.com:oidc:authentication_level:4:identification_level:3",
        "urn:onegini.com:oidc:authentication_level:4:identification_level:4"
    ],
    "code_challenge_methods_supported": [
        "plain",
        "S256"
    ]
}
```
