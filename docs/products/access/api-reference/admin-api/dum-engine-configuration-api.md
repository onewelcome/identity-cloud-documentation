# DUM Engine Configuration API

This allows the maintenance of the connections to DUM engine via a REST API. It can be utilized in scripts to add many clients at once, edit or delete them.

All endpoints are protected with the API client credentials (either Client Secret Basic or PrivateKeyJWT depending on the client [authentication method](../../topics/authentication-methods/authentication-methods.md)).
It requires an API client with the scope `onegini_api_admin` (Admin API).

## Endpoints

### List of DUM engine configurations

This returns a list of all DUM engine configurations. The `password` is never returned in the response.

* Endpoint: `/api/v1/configuration/dum-engines`
* Method: GET

Example request:

```http
GET /api/v1/configuration/dum-engines HTTP/1.1
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
         "id":"c7b34d6a-682e-4eb2-8d1d-af2842108867",
         "name":"DUM engine production cluster",
         "base_uri":"https://dum-prod.example.com",
         "username": "dum_user"
      }, 
      {
        … more DUM engine configurations …
      }
   ]
}
```

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Create DUM engine configuration

This creates a DUM engine configuration from scratch

* Endpoint: `/api/v1/configuration/dum-engines`
* Method: POST

JSON body parameters:

| Param                            | Required | Example                                              | Description                                                                  
|----------------------------------|----------|------------------------------------------------------|---------------------------------------------------------------------
| id                               |  yes     |  "c7b34d6a-682e-4eb2-8d1d-af2842108867"              |  Unique identifier for the configuration
| name                             |  yes     |  "DUM engine production cluster"                     |  Human readable name of the configuration
| base_uri                         |  yes     |  "https://dum-prod.example.com"                      |  Base URI that Onegini Access can reach for calls to DUM engine
| username                         |  yes     |  "dum_user"                                          |  Username to call the DUM engine APIs
| password                         |  yes     |  "AF33E2BF29C54A4639AB…"                             |  Password (not returned on GET)

  
Example request:

```http
POST /api/v1/configuration/dum-engines HTTP/1.1
Host: onegini.example.com
Content-Type: application/json
{
   "id":"c7b34d6a-682e-4eb2-8d1d-af2842108867",
   "name":"DUM engine production cluster",
   "base_uri":"https://dum-prod.example.com",
   "username": "dum_user"
   "password":"919724DAE12CAB220407C34EDAE8438CEAE965CD0F8AD033A743C1F4BB4B15C4",
}
```

Example success response:

```http
HTTP/1.1 201 CREATED
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
Location: /api/v1/configuration/dum-engines/c7b34d6a-682e-4eb2-8d1d-af2842108867
```

The success response body is empty. The `Location` header contains the URL for this new API client.

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Read DUM engine configuration

This returns a DUM engine configuration. The `password` is never returned in the response.

* Endpoint: `/api/v1/configuration/dum_engines/{id}`
* Method: GET

Path parameters:

| Param          | Required | Description                                                                  
|----------------|----------|------------------------------------------------------------------------------
| id             | yes      | Unique identifier of the DUM engine configuration


Example request:

```http
GET /api/v1/configuration/dum-engines/c7b34d6a-682e-4eb2-8d1d-af2842108867 HTTP/1.1
Host: onegini.example.com
```

Example success response:

```http
HTTP/1.1 200 Ok
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
{
   "id":"c7b34d6a-682e-4eb2-8d1d-af2842108867",
   "name":"DUM engine production cluster",
   "base_uri":"https://dum-prod.example.com",
   "username": "dum_user"
} 
```

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Update DUM engine configuration

Some fields can be updated after creating a DUM engine configuration.

* Endpoint: `/api/v1/configuration/dum-engines/{id}`
* Method: PATCH

Path parameters:

| Param          | Required | Description                                                                  
|----------------|----------|------------------------------------------------------------------------------
| id             | yes      | Unique identifier of the DUM engine configuration.


JSON body parameters:

Only the fields that are sent in the request will be changed.

| Param                            | Required | Example                                              | Description                                                                  
|----------------------------------|----------|------------------------------------------------------|---------------------------------------------------------------------
| name                             |  no      |  "DUM engine production cluster"                     |  Human readable name of the configuration
| base_uri                         |  no      |  "https://dum-prod.example.com"                      |  Base URI that Onegini Access can reach for calls to DUM engine
| username                         |  no      |  "dum_user"                                          |  Username to call the DUM engine APIs
| password                         |  no      |  "F167433E63CE8BD874D7…"                             |  Password (not returned on GET)
  
Example request:

```http
PATCH /api/v1/configuration/dum-engines/c7b34d6a-682e-4eb2-8d1d-af2842108867 HTTP/1.1
Host: onegini.example.com
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

### Delete DUM engine configuration

This removes a DUM engine configuration. This will also remove any link to this configuration, e.g. from the web client or application configuration.

* Endpoint: `/api/v1/configuration/dum-engines/{id}`
* Method: DELETE

Path parameters:

| Param          | Required | Description                                                                  
|----------------|----------|------------------------------------------------------------------------------
| id             | yes      | Unique identifier of the DUM engine configuration


Example request:

```http
DELETE /api/v1/configuration/dum-engines/c7b34d6a-682e-4eb2-8d1d-af2842108867 HTTP/1.1
Host: onegini.example.com
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
| 404         | not_found                        | DUM engine configuration cannot be found for this id
| 409         | conflict                         | The id already exists for a different DUM engine configuration
