# Web Clients API

This allows the creation of new Web clients via a REST API. It can be utilized in scripts to add many clients at once, edit or delete them.

All endpoints are protected with API client using either Client Secret Basic or PrivateKeyJWT [authentication method](../../topics/authentication-methods/authentication-methods.md). It requires an API client with the scope `onegini_api_config` (Config API).

## Endpoints

### List of Clients

This returns a list of all Web Clients.

* Endpoint: `/api/v1/configuration/web-clients`
* Method: GET

Request parameters:

| Param          | Required | Description
|----------------|----------|------------------------------------------------------------------------------
| page           | no       | Results are limited to 100 entries. Page number defaults to 0.

Example request:

```http
GET /api/v1/configuration/web-clients HTTP/1.1
Host: onegini.example.com
```

Example success response:

```http
HTTP/1.1 200 Ok
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
{
   "result":[
      {
         "name":"web client 1",
         "client_id":"365DADBA53849C3B67E7E3B736AA8C0701A98D6DC68047CD2AA10094DDFD835B",
         "client_authentication_method": "CLIENT_SECRET_BASIC",
         "grant_types":[
            "AUTHORIZATION_CODE"
         ],
         "access_token_format":"JWT",
         "redirect_url":"https://example.com/redirect",
         "additional_redirect_urls":[
            "https://example.com/redirect",
            "https://example.net/redirect"
         ],
         "access_grant_expires_in":30,
         "access_token_expires_in":3600,
         "additional_audiences":[],
         "refresh_token_enabled":true,
         "simultaneous_sessions_allowed":true,
         "max_simultaneous_sessions":25,
         "default_scopes":[
            "address",
            "email"
         ],
         "additional_scopes":[
            "phone"
         ],
         "identity_provider_id":"123-123",
         "additional_identity_provider_ids":["123-124", "123-125"],
         "logo_uri":"",
         "legacy_group_permissions_enabled": false,
         "template_set":"template1",
         "consent_disabled":true
      }, 
      {
        ... more clients ...
      }
   ]
}
```

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Create Web Client

This creates a Web Client from scratch

* Endpoint: `/api/v1/configuration/web-clients`
* Method: POST

JSON body parameters:

| Param                              | Required | Example                    | Description
|------------------------------------|----------|----------------------------|---------------------------------------------------------------------
| name                               |  yes     |  "myWebClient"             |  Custom client name
| client_id                          |  yes     |  "F167433E63CE8BD874D7…"   |  Unique Identifier for a client
| client_authentication_method       |  no      |  "CLIENT_SECRET_BASIC"     |  The authentication method that must be used for this client. Allowed values: `CLIENT_SECRET_BASIC`, `PKCE`. If no method is specified it will default to: `CLIENT_SECRET_BASIC`.
| client_secret                      |  depends |  "AF33E2BF29C54A4639AB…"   |  Client Secret (not returned on GET). Required when `client_authentication_method` is `CLIENT_SECRET_BASIC` or when `client_authentication_method` is not specified.
| public_jwk                         |  depends |   EC P-256 public key      |  Plain public key for private key JWT authentication. Onegini Access will favour `jwks_uri` if provided over statically defined JWK.
| jwks_uri                           |  depends |  "https://authorization-server/jwks  |  Uri of JWKS endpoint with public keys for private key JWT authentication.
| grant_types                        |  yes     |  ["CLIENT_CREDENTIALS"]    |  Set of Grant Types. Allowed values: `AUTHORIZATION_CODE`, `CLIENT_CREDENTIALS`. When `client_authentication_method` is `PKCE` the only allowed value is `AUTHORIZATION_CODE`.
| access_token_format                |  no      |  "OPAQUE"                  |  Format of the [Access Token](../../topics/tokens/access-token.md). Allowed values: `OPAQUE` (random string), `JWT` (JSON Web Token). Defaults to `OPAQUE` when omitted. The JWT access token contains the user identifier for grant types `AUTHORIZATION_CODE`.
| redirect_url                       |  yes     |  "https://example.com/redirect"   |  An URL to which the browser is redirected after successfully obtaining an Access grant. Required for Grant type `AUTHORIZATION_CODE`.
| additional_redirect_urls           |  no      |  ["https://example.org/redirect"] |  Additional URLs that the user can be redirected to after successfully obtaining an Access grant.
| access_grant_expires_in            |  yes     |  30                        |  The number of seconds that an Access grant is valid. Required for Grant type `AUTHORIZATION_CODE`.
| access_token_expires_in            |  yes     |  900                       |  The number of seconds that an Access token is valid. Required for Grant types `AUTHORIZATION_CODE` and `CLIENT_CREDENTIALS`.
| resource_gateway_ids               |  no      |  ["resource_gateway_1"]    |  An array with client_ids of existing clients that act as [Resource gateway](../../topics/general-app-config/resource-gateway/resource-gateway.md). It is used to populate the `aud` claim in a JWT Access Token.
| additional_audiences               |  no      |  ["aud2", "https://example.com"] |  An array of case sensitive strings that identifies the audience of the issued *access token* besides the resource gateways. It is used to populate the `aud` claim in a JWT Access Token.
| refresh_token_enabled              |  no      |  true                      |  Issue refresh tokens
| refresh_token_expires_in           |  no      |  7200                      |  The number of seconds that a refresh token is valid. 
| simultaneous_sessions_allowed      |  no      |  true                      |  Allow users to establish multiple sessions at the same time
| max_simultaneous_sessions          |  no      |  25                        |  Limits the number of sessions when simultaneous sessions are allowed. Value must be between 2 and 25 (inclusive). Defaults to 25 when omitted and `simultaneous_sessions_allowed` is `true`.
| default_scopes                     |  no      |  ["profile"]               |  Default scopes are the Scopes that are automatically assigned if a Client does not request any Scopes. If you want an Authorization request to fail when no Scopes are requested, leave this empty.
| additional_scopes                  |  no      |  ["address", "email"]      |  Additional Scopes that can be requested by a Client during an Authorization request. The Default scopes can also be requested by a Client during the Authorization request.
| identity_provider_id               |  no      |  "123-123"                 |  The Identifier of an existing [Identity Provider](../../topics/general-app-config/identity-providers/identity-providers.md) you wish to be the primary/default for this client. If none is specified in the request, this will be used.
| additional_identity_provider_ids   |  no      |  ["123-124", "123-125"]    |  Identifiers of additional existing Identity Providers that are able to be specified in the request.
| logo_uri                           |  no      |  "https://example.com/logo.png"   |  The URL that refers to the logo of the client.
| template_set                       |  no      |  "template1"               |  Identifier of an existing [Template Set](../../topics/look-and-feel/templates/templates.md)
| consent_disabled                   |  no      |  false                     |  When Consent is skipped, the user is not asked for Consent in the Authorization flow. The value must be `true` when `grant_types` contains `PASSWORD`.
| open_id_connect                    |  no      |  See table below           |  JSON Object for OpenID Connect configuration. Required when `default_scopes` or `additional_scopes` contain `openid`.
| legacy_group_permissions_enabled   |  no      |  false                     |  By default, the latest version of the DUM report is returned in claims. Set this flag value to `true` in order to use legacy version 1 of this report.

The following parameters are a part of the `open_id_connect` object.

| Param                              | Required | Example                                   | Description
|------------------------------------|----------|-------------------------------------------|---------------------------------------------------------------------
| expiration_time_seconds            | depends  | 3600                                      | The number of seconds that an ID Token is valid. It is used to calculate the `exp` expiration date claim in the ID Token. Required when `default_scopes` or `additional_scopes` contains `openid`.
| additional_audiences               | no       | ["https://resource.example.com"]          | An array of case sensitive strings that identifies who is the audience of the issued *ID Token*. It is used to populate `aud` claim in the ID Token in combination with the `client_id` which is always included.
| post_logout_redirect_url           | no       | "https://redirect.example.com"            | This is Primary/Default Post Logout URL. The user will be redirected to this URL if no other URL is specified in the end session request.
| additional_post_logout_redirect_urls | no     | ["https://redirect.example.com", "https://postlogout.example.com"]  | An array of additional Post Logout Redirect URLs. This array contains the potential URLs that could be specified to be redirected to as part of the end session request.
| front_channel_logout_url           | no       | "https://front-channel-logout.example.com"  | A URL reachable on the Relying Party that will invalidate the session. This will be triggered after a successful logout via the end session page.
| id_token_encryption_enabled        | no       |                                           | Enabling/Disabling ID Token encryption
| id_token_encryption_method         | depends  | "A256GCM"                                 | Encryption Method used to encrypt ID Token. Required when `id_token_encryption_enabled` is `true`. Allowed values: `A128GCM`, `A192GCM`, `A256GCM`, `A128CBC-HS256`, `A192CBC-HS384`, `A256CBC-HS512`.
| id_token_jwks_uri                  | depends  | "https://example.com/jwks.json"           | JSON Web Key Set (JWKS) endpoint which contains JSON Web Keys (JWK) used to encrypt the ID Token. Required when `id_token_encryption_enabled` is `true`.


Example request:

```http
POST /api/v1/configuration/web-clients HTTP/1.1
Host: onegini.example.com
Content-Type: application/json
{
  "name": "web client 1",
  "client_id": "365DADBA53849C3B67E7E3B736AA8C0701A98D6DC68047CD2AA10094DDFD835B",
  "client_secret": "919724DAE12CAB220407C34EDAE8438CEAE965CD0F8AD033A743C1F4BB4B15C4",
  "client_authentication_method": "CLIENT_SECRET_BASIC",
  "grant_types": [
    "AUTHORIZATION_CODE",
    "CLIENT_CREDENTIALS"
  ],
  "access_token_format": "JWT",
  "redirect_url": "https://example.com/redirect",
  "additional_redirect_urls": [
    "https://example.org/redirect",
    "https://example.net/redirect"
  ],
  "access_grant_expires_in": 30,
  "access_token_expires_in": 3600,
  "refresh_token_enabled": true,
  "simultaneous_sessions_allowed": true,
  "max_simultaneous_sessions": 25,
  "default_scopes": [
    "address",
    "email"
  ],
  "additional_scopes": [
    "phone",
    "openid"
  ],
  "identity_provider_id": "123-123",
  "consent_disabled": true,
  "legacy_group_permissions_enabled": true,
  "open_id_connect": {
    "expiration_time_seconds": 3600,
    "post_logout_redirect_url": "https://redirect.example.com"
  }
}
```

Example success response:

```http
HTTP/1.1 201 CREATED
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
Location: /api/v1/configuration/web-clients/365DADBA53849C3B67E7E3B736AA8C0701A98D6DC68047CD2AA10094DDFD835B
```

The success response body is empty.

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Read Web Client

This returns one Web Client.

* Endpoint: `/api/v1/configuration/web-clients/{client_id}`
* Method: GET

Path parameters:

| Param          | Required | Description
|----------------|----------|------------------------------------------------------------------------------
| client_id      | yes      | Unique identifier of the Web Client.

Example request:

```http
GET /api/v1/configuration/web-clients/365DADBA53849C3B67E7E3B736AA8C0701A98D6DC68047CD2AA10094DDFD835B HTTP/1.1
Host: onegini.example.com
```

Example success response:

```http
HTTP/1.1 200 Ok
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
{
   "name":"web client 1",
   "client_id":"365DADBA53849C3B67E7E3B736AA8C0701A98D6DC68047CD2AA10094DDFD835B",
   "client_authentication_method": "CLIENT_SECRET_BASIC",
   "grant_types":[
      "AUTHORIZATION_CODE"
   ],
   "access_token_format":"JWT",
   "redirect_url":"https://example.com/redirect",
   "additional_redirect_urls":[
      "https://example.org/redirect",
      "https://example.net/redirect"
   ],
   "access_grant_expires_in":30,
   "access_token_expires_in":3600,
   "additional_audiences":[],
   "refresh_token_enabled":true,
   "simultaneous_sessions_allowed":true,
   "default_scopes":[
      "address",
      "email"
   ],
   "additional_scopes":[
      "phone"
   ],
   "identity_provider_id":"123-123",
   "additional_identity_provider_ids":["123-124", "123-125"],
   "logo_uri":"",
   "template_set":"template1",
   "consent_disabled":true,
  "legacy_group_permissions_enabled": true
}
```

### Update Web Client

Some fields can be updated after creating a Web Client

* Endpoint: `/api/v1/configuration/web-clients/{client_id}`
* Method: PATCH

Path parameters:

| Param          | Required | Description
|----------------|----------|------------------------------------------------------------------------------
| client_id      | yes      | Unique identifier of the Web Client.


JSON body parameters:

Only the fields that are sent in the request will be changed.

| Param                              | Required | Example                    | Description
|------------------------------------|----------|----------------------------|---------------------------------------------------------------------
| name                               |  no      |  "My web portal"           |  Client name
| client_authentication_method       |  no      |  "CLIENT_SECRET_BASIC"     |  The authentication method that must be used for this client. Allowed values: `CLIENT_SECRET_BASIC`, `PKCE`. If no method is specified it will default to: `CLIENT_SECRET_BASIC`.
| client_secret                      |  no      |  "AF33E2BF29C54A4639AB…"   |  Client Secret (not returned on GET)
| public_jwk                         |  depends |   EC P-256 public key      |  Plain public key for private key JWT authentication. The Onegini Access will favour `jwks_uri` if provided over statically defined JWK.
| jwks_uri                           |  depends |  "https://authorization-server/jwks  |  Uri of JWKS endpoint with public keys for private key JWT authentication.
| grant_types                        |  no      |  ["CLIENT_CREDENTIALS"]    |  Set of Grant Types. Allowed values: `AUTHORIZATION_CODE`, `CLIENT_CREDENTIALS`.  When `client_authentication_method` is `PKCE` the only allowed value is `AUTHORIZATION_CODE`.
| access_token_format                |  no      |  "OPAQUE"                  |  Format of the [Access Token](../../topics/tokens/access-token.md). Allowed values: `OPAQUE` (random string), `JWT` (JSON Web Token). The JWT access token contains the user identifier for grant type `AUTHORIZATION_CODE`.
| redirect_url                       |  no      |  "https://example.com/redirect"   |  An URL to which the browser is redirected after successfully obtaining an Access grant. Used with Grant type `AUTHORIZATION_CODE`.
| additional_redirect_urls           |  no      |  ["https://example.org/redirect"] |  Additional URLs that the user can be redirected to after successfully obtaining an Access grant.
| access_grant_expires_in            |  no      |  30                        |  The number of seconds that an Access grant is valid. Used with Grant type `AUTHORIZATION_CODE`.
| access_token_expires_in            |  no      |  3600                      |  The number of seconds that an Access token is valid. Used with Grant types `AUTHORIZATION_CODE`, `CLIENT_CREDENTIALS`.
| resource_gateway_ids               |  no      |  ["resource_gateway_1"]    |  An array with client_ids of existing clients that act as [Resource gateway](../../topics/general-app-config/resource-gateway/resource-gateway.md). It is used to populate the `aud` claim in a JWT Access Token.
| additional_audiences               |  no      |  ["aud2", "https://example.com"] |  An array of case sensitive strings that identifies the audience of the issued access token besides the resource gateways. It is used to populate the `aud` claim in a JWT Access Token.
| refresh_token_enabled              |  no      |  true                      |  Issue refresh tokens
| refresh_token_expires_in           |  no      |  7200                      |  The number of seconds that a refresh token is valid. 
| simultaneous_sessions_allowed      |  no      |  true                      |  Allow users to establish multiple sessions at the same time
| max_simultaneous_sessions          |  no      |  25                        |  Limits the number of sessions when simultaneous sessions are allowed. Value must be between 2 and 25 (inclusive). Defaults to 25 when omitted and `simultaneous_sessions_allowed` is `true`.
| default_scopes                     |  no      |  ["profile"]               |  Default scopes are the Scopes that are automatically assigned if a Client does not request any Scopes. If you want an Authorization request to fail when no Scopes are requested, leave this empty.
| additional_scopes                  |  no      |  ["address", "email"]      |  Additional Scopes that can be requested by a Client during an Authorization request. The Default scopes can also be requested by a Client during the Authorization request.
| identity_provider_id               |  no      |  "123-123"                 |  The Identifier of an existing [Identity Provider](../../topics/general-app-config/identity-providers/identity-providers.md) you wish to be the primary/default for this client.
| additional_identity_provider_ids   |  no      |  ["123-124", "123-125"]    |  Identifiers of additional existing Identity Providers that are able to be specified in the request.
| logo_uri                           |  no      |  "https://example.com/logo.png"  |  The URL that refers to the logo of the client.
| template_set                       |  no      |  "template1"               |  Identifier of existing [Template Set](../../topics/look-and-feel/templates/templates.md)
| consent_disabled                   |  no      |  false                     |  When Consent is skipped, the user is not asked for Consent in the Authorization flow
| open_id_connect                    |  no      |  See table below           |  JSON Object for OpenID Connect configuration. Required when `openid` is added to `default_scopes` or `additional_scopes`.
| legacy_group_permissions_enabled   |  no      |  false                     |  By default, the latest version of the DUM report is returned in claims. Set this flag value to `true` in order to use legacy version 1 of this report.

The following parameters are a part of the `open_id_connect` object.

| Param                              | Required | Example                                   | Description
|------------------------------------|----------|-------------------------------------------|---------------------------------------------------------------------
| expiration_time_seconds            | depends  |  3600                                     |  The number of seconds that an ID Token is valid. It is used to calculate the `exp` expiration date claim in the ID Token. Required when `openid` is added to `default_scopes` or `additional_scopes`.
| additional_audiences               | no       | ["https://resource.example.com"]          | An array of case sensitive strings that identifies who is the audience of the issued *ID Token*. It is used to populate `aud` claim in the ID Token in combination with the `client_id` which is always included.
| post_logout_redirect_url           | no       | "https://redirect.example.com"            | This is Primary/Default Post Logout URL. The user will be redirected to this URL if no other URL is specified in the end session request.
| additional_post_logout_redirect_urls | no     | ["https://redirect.example.com", "https://postlogout.example.com"]  | An array of additional Post Logout Redirect URLs. This array contains the potential URLs that could be specified to be redirected to as part of the end session request.
| front_channel_logout_url           | no       | "https://front-channel-logout.example.com"  | A URL reachable on the Relying Party that will invalidate the session. This will be triggered after a successful logout via the end session page.
| id_token_encryption_enabled        | no       |                                           | Enabling/Disabling ID Token encryption
| id_token_encryption_method         | depends  | "A256GCM"                                 | Encryption Method used to encrypt ID Token. Required when `id_token_encryption_enabled` is changed to `true`. Allowed values: `A128GCM`, `A192GCM`, `A256GCM`, `A128CBC-HS256`, `A192CBC-HS384`, `A256CBC-HS512`.
| id_token_jwks_uri                  | depends  | "https://example.com/jwks.json"           | JSON Web Key Set (JWKS) endpoint which contains JSON Web Keys (JWK) used to encrypt the ID Token. Required when `id_token_encryption_enabled` is changed to `true`.


Example request:

```http
PATCH /api/v1/configuration/web-clients/F167433E63CE8BD874D7 HTTP/1.1
Host: onegini.example.com
Content-Type: application/json

{
  "scopes": ["address", "email", "phone"]
}
```

Example success response:

```http
HTTP/1.1 204 NO CONTENT
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
```

The success response body is empty.

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Delete Web Client

This removes a Web Client.

* Endpoint: `/api/v1/configuration/web-clients/{client_id}`
* Method: DELETE

Path parameters:

| Param          | Required | Description
|----------------|----------|------------------------------------------------------------------------------
| client_id      | yes      | Unique identifier of the Web Client.


Example request:

```http
DELETE /api/v1/configuration/web-clients/365DADBA53849C3B67E7E3B736AA8C0701A98D6DC68047CD2AA10094DDFD835B HTTP/1.1
Host: onegini.example.com
```

Example success response:

```http
HTTP/1.1 204 NO CONTENT
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
```


## Error codes

One of the following responses will be returned, containing a JSON object with an error code, a message and details about the error.

| HTTP status | Error code                       | Message
|-------------|----------------------------------|-------------------------------------------------------------------------------------
| 400         | invalid_request                  | One or more parameters is missing or incorrect. The details contain the missing or incorrect parameters.
| 401         | unauthorized                     | Provide valid credentials to get access to the API.
| 403         | forbidden                        | Operation is not allowed for the current user.
| 404         | not_found                        | Web Client configuration cannot be found for this client_id
| 409         | conflict                         | The Client ID already exists

## Limitations

Some fields are references to other parts in the configuration of Onegini Access. This API can only refer to existing identifiers for the following:

  * Resource gateways
  * Identity Providers (both primary and additional)
  * Scopes (both default and additional)
  * Template Set

When an unknown identifier is passed, this API returns an HTTP status code 400 with error code invalid_request.
