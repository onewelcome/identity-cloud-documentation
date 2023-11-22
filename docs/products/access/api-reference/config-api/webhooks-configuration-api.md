# Web Hooks Configuration API

This allows the maintenance of the WebHook definitions that can be later used by the OAuth Clients. The WebHooks can serve different purposes ranging from 
making access decisions to user details manipulation.  

All endpoints are protected with the API client credentials (either Client Secret Basic or PrivateKeyJWT depending on the client's authentication method).
It requires an API client with the scope `onegini_api_admin` (Admin API).

## Endpoints

### List of Web Hook configurations

This returns a list of all Web Hook configurations. The `password` is never returned with the response.

* Endpoint: `/api/v1/configuration/web-hooks`
* Method: GET

Example request:

```http
GET /api/v1/configuration/web-hooks HTTP/1.1
Host: example.onewelcome.com
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
         "id":"c7b34d6a-682e-4eb2-8d1d-af2842108867",
         "type":"DABP",
         "name":"Delegated Administration Web Hook production cluster",
         "base_uri":"https://dabp-prod.onewelcome.com",
         "authentication_method":"BASIC",
         "username": "dabp_user"
      }, 
      {
        … more Web Hook configurations …
      }
   ]
}
```

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Create Web Hook configuration

This creates a Web Hook configuration from scratch

* Endpoint: `/api/v1/configuration/web-hooks`
* Method: POST

JSON body parameters:

| Param                            | Required | Example                                              | Description                                                                  
|----------------------------------|----------|------------------------------------------------------|---------------------------------------------------------------------
| id                               |  no      |  "c7b34d6a-682e-4eb2-8d1d-af2842108867"              |  Unique identifier for the configuration
| type                             |  yes     |  "DABP"                                              |  The type of the WebHook, allowed values are `CUSTOMIZE_TOKEN`, `DABP` and `USER_DETAILS_CUSTOMIZATION`
| name                             |  yes     |  "Delegated Administration production cluster"       |  Human readable name of the configuration
| base_uri                         |  yes     |  "https://dabp-prod.example.com"                     |  Base URI that Access Service can reach for calls to Web Hook API
| authentication_method            |  yes     |  "BASIC"                                             |  The type of the authentication required by the WebHook, allowed values are `BASIC` and `NONE`.
| username                         |  no      |  "dabp_user"                                         |  Username to call the Web Hook API. Required when authentication method is `BASIC`. 
| password                         |  no      |  "AF33E2BF29C54A4639AB…"                             |  Password (not returned on GET). Required when authentication method is `BASIC`.

  
Example request:

```http
POST /api/v1/configuration/web-hooks HTTP/1.1
Host: example.onewelcome.com
Content-Type: application/json
{
   "id":"c7b34d6a-682e-4eb2-8d1d-af2842108867",
   "type:"USER_DETAILS_CUSTOMIZATION",
   "name":"User Details Customization Web Hook",
   "base_uri":"https://customize-user-details.onewelcome.com",
   "authentication_method": "NONE"
}
```

Example success response:

```http
HTTP/1.1 201 CREATED
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
Location: /api/v1/configuration/web-hooks/c7b34d6a-682e-4eb2-8d1d-af2842108867
```

Example request:

```http
POST /api/v1/configuration/web-hooks HTTP/1.1
Host: example.onewelcome.com
Content-Type: application/json
{
   "id":"c7b34d6a-682e-4eb2-8d1d-af2842108868",
   "type:"CUSTOMIZE_TOKEN",
   "name":"Customize Token Web Hook",
   "base_uri":"https://customize-token.onewelcome.com",
   "authentication_method": "NONE"
}
```

Example success response:

```http
HTTP/1.1 201 CREATED
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
Location: /api/v1/configuration/web-hooks/c7b34d6a-682e-4eb2-8d1d-af2842108868
```

The success response body is empty. The `Location` header contains the URL for this new API client.

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Read Web Hook configuration

This returns a Web Hook configuration. The sensitive authentication credentials like `password` are never returned with the response.

* Endpoint: `/api/v1/configuration/web-hooks/{id}`
* Method: GET

Path parameters:

| Param          | Required | Description                                                                  
|----------------|----------|------------------------------------------------------------------------------
| id             | yes      | Unique identifier of the Web Hook configuration


Example request:

```http
GET /api/v1/configuration/web-hooks/c7b34d6a-682e-4eb2-8d1d-af2842108867 HTTP/1.1
Host: example.onewelcome.com
```

Example success response:

```http
HTTP/1.1 200 Ok
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
{
   "id":"c7b34d6a-682e-4eb2-8d1d-af2842108867",
   "type":"DABP",
   "name":"Delegated Administration production cluster",
   "base_uri":"https://dabp-prod.onewelcome.com",
   "authentication_method":"BASIC",
   "username": "dabp_user"
} 
```

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Update Web Hook configuration

Some fields can be updated after creating a Web Hook configuration.

* Endpoint: `/api/v1/configuration/web-hooks/{id}`
* Method: PATCH

Path parameters:

| Param          | Required | Description                                                                  
|----------------|----------|------------------------------------------------------------------------------
| id             | yes      | Unique identifier of the Web Hook configuration.


JSON body parameters:

Only the fields that are sent in the request will be changed.

| Param                            | Required | Example                                              | Description                                                                  
|----------------------------------|----------|------------------------------------------------------|---------------------------------------------------------------------
| name                             |  no      |  "Delegated Administration production cluster"       |  Human readable name of the configuration
| type                             |  no      |  "DABP"                                              |  The type of the WebHook, allowed values are `CUSTOMIZE_TOKEN`, `DABP` and `USER_DETAILS_CUSTOMIZATION`
| base_uri                         |  no      |  "https://dabp-prod.example.com"                     |  Base URI that Access Service can reach for calls to DUM engine
| authentication_method            |  no      |  "BASIC"                                             |  The type of the authentication required by the WebHook, allowed values are `BASIC` and `NONE`.
| username                         |  no      |  "dabp_user"                                         |  Username to call the DUM engine APIs
| password                         |  no      |  "F167433E63CE8BD874D7…"                             |  Password (not returned on GET)
  
Example request:

```http
PATCH /api/v1/configuration/web-hooks/c7b34d6a-682e-4eb2-8d1d-af2842108867 HTTP/1.1
Host: example.onewelcome.com
Content-Type: application/json

{
  "password":"F167433E63CE8BD874D7F167433E63CE8BD874D7"
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

### Delete Web Hook configuration

This removes a Web Hook configuration. This will also remove any link to this configuration, e.g. from the web client or application configuration.

* Endpoint: `/api/v1/configuration/web-hooks/{id}`
* Method: DELETE

Path parameters:

| Param          | Required | Description                                                                  
|----------------|----------|------------------------------------------------------------------------------
| id             | yes      | Unique identifier of the Web Hook configuration


Example request:

```http
DELETE /api/v1/configuration/web-hooks/c7b34d6a-682e-4eb2-8d1d-af2842108867 HTTP/1.1
Host: example.onewelcome.com
```

Example success response:

```http
HTTP/1.1 204 NO CONTENT
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
```

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

## Error codes

One of the following responses will be returned, containing a JSON object with an error code, a message and details about the error.

| HTTP status | Error code                       | Message 
|-------------|----------------------------------|-------------------------------------------------------------------------------------
| 400         | invalid_request                  | One or more parameters is missing or incorrect. The details contain the missing or incorrect parameters.
| 401         | unauthorized                     | Provide valid credentials to get access to the API.
| 403         | forbidden                        | Operation is not allowed for the current user. 
| 404         | not_found                        | Web Hook configuration cannot be found for this id
| 409         | conflict                         | The id already exists for a different Web Hook configuration
