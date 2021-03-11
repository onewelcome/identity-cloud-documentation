# API Clients API

This allows the creation of new API clients via a REST API. It can be utilized in scripts to add many clients at once, edit or delete them.

All endpoints are protected with API client credentials (either Client Secret Basic or PrivateKeyJWT depending on the client [authentication method](../../topics/authentication-methods/authentication-methods.md). It requires an API client with the scope `onegini_api_admin` (Admin API).

## Initial API client

In order to use this API there must be an API client configured that grants you access it. Such an API client can be created via the [Admin console](../../topics/technical-app-management/api-configuration/api-configuration.md).

## Endpoints

### List of API Clients

This returns a list of all API Clients. The `client_secret` is never returned in the response.

* Endpoint: `/api/v1/configuration/api-clients`
* Method: GET

Request parameters:

| Param          | Required | Description                                                                  
|----------------|----------|------------------------------------------------------------------------------
| page           | no       | Results are limited to 100 entries. Page number defaults to 0.


Example request:

```http
GET /api/v1/configuration/api-clients?page=1 HTTP/1.1
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
         "name":"API client 1",
         "client_id":"365DADBA53849C3B67E7E3B736AA8C0701A98D6DC68047CD2AA10094DDFD835B",
         "scopes":[
          "onegini_api_end_user",
          "onegini_api_user_registration"
         ],
         "public_base_uri":""
      }, 
      {
        … more api clients …
      }
   ]
}
```

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Create API Client

This creates an API Client from scratch

* Endpoint: `/api/v1/configuration/api-clients`
* Method: POST

JSON body parameters:

| Param                            | Required     | Example                                              | Description                                                                  
|----------------------------------|--------------|------------------------------------------------------|---------------------------------------------------------------------
| name                             |  yes         |  "API client 1"                                      |  Client name
| client_id                        |  yes         |  "F167433E63CE8BD874D7…"                             |  Unique identifier for a client
| authentication_method            |  no          |  "private_key_jwt"                                   |  Indicates authentication method for that client. Options are `client_secret_basic` and `private_key_jwt`. If not provided it defaults to `client_secret_basic`.
| client_secret                    |  depends     |  "AF33E2BF29C54A4639AB…"                             |  Client secret (not returned on GET)
| public_jwk                       |  depends     |   EC P-256 public key                                |  Plain public key for private key JWT authentication. Onegini Access will favour `jwks_uri` if provided over statically defined JWK.
| jwks_uri                         |  depends     |  "https://authorization-server/jwks"                 |  Uri of JWKS endpoint with public keys for private key JWT authentication.
| scopes                           |  yes         |  ["onegini_api_end_user", "onegini_api_user_registration"] |  Valid values are described in the [API scopes](../../appendix/administration/api-config.md#API-scopes)
| public_base_uri                  |  no          |  "https://example.com/sth"                           |  When this Client gives access to the [Token introspection API](../token-introspection.md), it has some URI where it can be reached. Configure the base URI in which all calls to this client should originate.
  
Example request:

```http
POST /api/v1/configuration/api-clients HTTP/1.1
Host: onegini.example.com
Content-Type: application/json
{
   "name":"API client 1",
   "client_id":"365DADBA53849C3B67E7E3B736AA8C0701A98D6DC68047CD2AA10094DDFD835B",
   "client_secret":"919724DAE12CAB220407C34EDAE8438CEAE965CD0F8AD033A743C1F4BB4B15C4",
   "scopes":[
      "onegini_api_end_user",
      "onegini_api_user_registration"
   ],
   "public_base_uri":""
}
```

Example success response:

```http
HTTP/1.1 201 CREATED
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
Location: /api/v1/configuration/api-clients/365DADBA53849C3B67E7E3B736AA8C0701A98D6DC68047CD2AA10094DDFD835B
```

The success response body is empty. The `Location` header contains the URL for this new API client.

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Read API Client

This returns an API Client. The `client_secret` is never returned in the response.

* Endpoint: `/api/v1/configuration/api_clients/{client_id}`
* Method: GET

Path parameters:

| Param          | Required | Description                                                                  
|----------------|----------|------------------------------------------------------------------------------
| client_id      | yes      | Unique identifier of the API Client.


Example request:

```http
GET /api/v1/configuration/api-clients/365DADBA53849C3B67E7E3B736AA8C0701A98D6DC68047CD2AA10094DDFD835B HTTP/1.1
Host: onegini.example.com
```

Example success response:

```http
HTTP/1.1 200 Ok
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
{
   "name":"API client 1",
   "client_id":"365DADBA53849C3B67E7E3B736AA8C0701A98D6DC68047CD2AA10094DDFD835B",
   "scopes":[
      "onegini_api_end_user",
      "onegini_api_user_registration"
   ],
   "public_base_uri":""
}, 
```

### Update API Client

Some fields can be updated after creating an API Client

* Endpoint: `/api/v1/configuration/api-clients/{client_id}`
* Method: PATCH

Path parameters:

| Param          | Required | Description                                                                  
|----------------|----------|------------------------------------------------------------------------------
| client_id      | yes      | Unique identifier of the API Client.


JSON body parameters:

Only the fields that are sent in the request will be changed.

| Param                            | Required     | Example                                              | Description                                                                  
|----------------------------------|--------------|------------------------------------------------------|---------------------------------------------------------------------
| name                             |  no          |  "API client 1"                                      |  Custom client name
| authentication_method            |  no          |  "private_key_jwt"                                   |  Indicates authentication method fot that client. Options are `client_secret_basic` and `private_key_jwt`. If not provided it defaults to `client_secret_basic`
| client_secret                    |  depends     |  "AF33E2BF29C54A4639AB…"                             |  Client secret (not returned on GET)
| scopes                           |  no          |  ["onegini_api_end_user", "onegini_api_user_registration"] |  Valid values are described in the [API scopes](../../appendix/administration/api-config.md#API-scopes)
| public_base_uri                  |  no          |  "https://example.com/sth"                           |  When this Client gives access to the [Token introspection API](../token-introspection.md), it has some URI where it can be reached. Configure the base URI in which all calls to this client should originate.
| public_jwk                       |  depends     |   EC P-256 public key                                |  Plain public key for private key JWT authentication. Onegini Access will favour `jwks_uri` if provided over statically defined JWK.
| jwks_uri                         |  depends     |  "https://authorization-server/jwks                  |  Uri of JWKS endpoint with public keys for private key JWT authentication.
  
Example request:

```http
PATCH /api/v1/configuration/api-clients/F167433E63CE8BD874D7 HTTP/1.1
Host: onegini.example.com
Content-Type: application/json

{
  "scopes": ["onegini_api_end_user", "onegini_api_mobile_authentication"]
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

### Delete API Client

This removes an API Client.

* Endpoint: `/api/v1/configuration/api-clients/{client_id}`
* Method: DELETE

Path parameters:

| Param          | Required | Description                                                                  
|----------------|----------|------------------------------------------------------------------------------
| client_id      | yes      | Unique identifier of the API Client.


Example request:

```http
DELETE /api/v1/configuration/api-clients/365DADBA53849C3B67E7E3B736AA8C0701A98D6DC68047CD2AA10094DDFD835B HTTP/1.1
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
| 404         | not_found                        | "API Client" configuration cannot be found for this client_id
| 409         | conflict                         | The Client ID already exists for a different API client, [Web client](../config-api/web-client.md) or [Device](../end-user/device-v4.md)
