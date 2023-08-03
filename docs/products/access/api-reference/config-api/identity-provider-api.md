# Identity Providers

These APIs allow the retrieval of configuration
of [identity-providers](../../topics/general-app-config/identity-providers/identity-providers.md) via a REST
API.

## Endpoints

All endpoints are protected with the API client credentials (either client secret basic or private key JWT depending on the client
[authentication method](../../topics/authentication-methods/authentication-methods.md). It requires an
[API client](../../appendix/administration/api-config.md) with the scope `Config API`.

### Get a specific Identity Provider

This returns the details of a specific Identity Provider.

* Endpoint: `/api/v1/configuration/idps/{idpId}/`
* Method: GET
* **Please note that `/` at the end is required**

Path parameters:

| Param | Required | Description                                 |
|-------|----------|---------------------------------------------|
| idpId | yes      | Unique identifier of the Identity Provider. |

Example request:

```http
GET /api/v1/configuration/idps/customIdp/ HTTP/1.1
Host: onegini.example.com
Content-Type: application/json
```

Example success response:

```http
HTTP/1.1 200 Ok
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache

{
   "id": "customIdp",
   "name": "Custom Identity Provider",
   "type": "CUSTOM",
   "enabled": true,
   "default": true
}
```

### Get all identity providers

This returns all configured identity providers.

* Endpoint: `/api/v1/configuration/idps`
* Method: GET

Example request:

```http
GET /api/v1/configuration/idps HTTP/1.1
Host: onegini.example.com
Content-Type: application/json
```

Example success response:

```http
HTTP/1.1 200 Ok
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache

{
    "result": [
        {
            "id": "cimapi",
            "name": "cimapi",
            "type": "ONEGINI_API_ONE_STEP",
            "enabled": true,
            "default": false
        },
        {
            "id": "oneginiidp",
            "name": "oneginiidp",
            "type": "ONEGINI",
            "enabled": true,
            "default": false
        }
    ]
}
```

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Create Identity Provider

This creates an Identity Provider from scratch

* Endpoint: `/api/v1/configuration/idps`
* Method: POST

Supported Identity Provider types: `TULIP`, `OAUTH`

JSON body parameters:

| Param                        | Idp type     | Required | Description                                                                                                                                                                                                             |
|------------------------------|--------------|----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                           | all          | yes      | Unique identifier for an Identity Provider.                                                                                                                                                                             |
| name                         | all          | yes      | Unique name of an Identity Provider.                                                                                                                                                                                    |
| type                         | all          | yes      | Identity Provider type. <br/> Supported types: `TULIP`, `OAUTH`.                                                                                                                                                        |
| enabled                      | all          | no       | Specify whether an Identity Provider is enabled. <br/> Default value: `true`.                                                                                                                                           |
| default                      | all          | no       | Specify whether an Identity Provider is default. <br/> Default value: `false`.                                                                                                                                          |
| issuer_uri                   | TULIP        | yes      | Uri of the issuer. This URI will be used to read the OpenID Connect configuration.                                                                                                                                      |
| client_id                    | TULIP, OAUTH | yes      | Client identifier.                                                                                                                                                                                                      |
| client_secret                | TULIP, OAUTH | depends  | Client secret. <br/> Required if client authentication method is `client_secret_basic` or `client_secret_post`                                                                                                          |
| client_authentication_method | TULIP, OAUTH | no       | Client authentication method. <br/> Supported values: `private_key_jwt`, `client_secret_basic`, `client_secret_post`. <br/> Default value is `private_key_jwt`.                                                         |
| scopes                       | TULIP, OAUTH | no       | Space-separated scopes.                                                                                                                                                                                                 |
| end_session_enabled          | TULIP        | no       | Specify whether End Session integration is enabled for this Identity Provider. <br/> Default value: `false`.                                                                                                            |
| integrations                 | TULIP        | no       | List of enabled integrations. <br/> Supported values: `APP_TO_WEB`, `UDH_API`.                                                                                                                                          |
| tulip_api_client_id          | TULIP        | depends  | Client identifier for Tulip API calls. <br/> Required when `APP_TO_WEB` or `UDH_API` integration is enabled.                                                                                                            |
| tulip_api_client_secret      | TULIP        | depends  | Client secret for Tulip API calls. <br/> Required when `APP_TO_WEB` or `UDH_API` integration is enabled and authentication method is `client_secret_basic` or `client_secret_post`.                                     |
| tulip_api_base_url           | TULIP        | depends  | This should be the base url of the Tulip brand without a trailing slash. UDH and App To Web will use this as a base for their urls. <br/> Required when `APP_TO_WEB` or `UDH_API` integration is enabled.               |
| tulip_api_access_scope       | TULIP        | depends  | Space-separated scopes for the required Tulip segments e.g. `iwelcome:segment:example`. <br/> Required when `APP_TO_WEB` or `UDH_API` integration is enabled.                                                           |
| tulip_api_used_auth_methods  | TULIP        | no       | List of Auth Methods for the App to Web integration with Tulip e.g. `["SMS", "another"]`. <br/> Used when `APP_TO_WEB` integration is enabled.                                                                        |
| authorization_url            | OAUTH        | yes      | Oauth authorization endpoint.                                                                                                                                                                                           |
| token_url                    | OAUTH        | yes      | Oauth token endpoint.                                                                                                                                                                                                   |
| profile_url                  | OAUTH        | yes      | OpenID Connect UserInfo endpoint.                                                                                                                                                                                       |
| user_info_enabled            | OAUTH        | no       | Specify whether CIM's Person API is enabled for this Identity Provider. <br/> Default value: `false`.                                                                                                                   |
| user_info_endpoint           | OAUTH        | depends  | Identity source URL. The URL of API that provides user's identity. Use `{userId}` placeholder for userId path param. e.g. https://host/api/persons/{userId}/profile  <br/> Required when `user_info_enabled` is `true`. |
| user_info_username           | OAUTH        | depends  | Identity source username. <br/> Required when `user_info_enabled` is `true`.                                                                                                                                            |
| user_info_password           | OAUTH        | depends  | Identity source password. <br/> Required when `user_info_enabled` is `true`.                                                                                                                                            |

Example `TULIP` type request:

```http
POST /api/v1/configuration/idps 
Host: onegini.example.com
Content-Type: application/json
{
   "id": "tulip-idp",
   "name": "Tulip Idp",
   "type": "TULIP",
   "client_id": "oauth2CustomerApp",
   "issuer_uri": "https://tulip.onewelcome.com/segment/login/oauth2/v1",
   "end_session_enabled": true,
   "scopes": "openid",
   "integrations": [
      "APP_TO_WEB",
      "UDH_API"
   ],
   "tulip_api_client_id": "accessIntegration",
   "tulip_api_base_url": "https://tulip-api.onewelcome.com/segment",
   "tulip_api_access_scope": "iwelcome:segment:example",
   "tulip_api_used_access_methods": [
        "SMS",
        "another"
    ]
}
```

Example `OAUTH` type request:

```http
POST /api/v1/configuration/idps 
Host: onegini.example.com
Content-Type: application/json
{
  "type": "OAUTH",
  "id": "oauth-idp",
  "name": "OAuth Identity Provider",
  "client_id": "oauth2CustomerApp",
  "authorization_url": "https://example.com/oauth/v1/authorize",
  "token_url": "https://example.com/oauth/v1/token",
  "profile_url": "https://example.com/oauth/v1/userinfo",
  "scopes": "openid profile email"
}
```

Example success response:

```http
HTTP/1.1 201 CREATED
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
Location: /api/v1/configuration/idps/tulip-insurcar
```

The success response body is empty.

### Delete Identity Provider

This removes an Identity Provider.

* Endpoint: `/api/v1/configuration/idps/{id}`
* Method: DELETE

Path parameters:

| Param     | Required | Description                                 |
|-----------|----------|---------------------------------------------|
| id        | yes      | Unique identifier for an Identity Provider. |

Example request:

```http
DELETE /api/v1/configuration/idps/oneginiidp HTTP/1.1
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

| HTTP status | Error code      | Message                                                                                                  |
|-------------|-----------------|----------------------------------------------------------------------------------------------------------|
| 400         | invalid_request | One or more parameters is missing or incorrect. The details contain the missing or incorrect parameters. |
| 401         | unauthorized    | Provide valid credentials to get access to the API.                                                      |
| 403         | forbidden       | Operation is not allowed for the current user.                                                           |
| 404         | not_found       | Identity Provider for given ID cannot be found                                                           |
