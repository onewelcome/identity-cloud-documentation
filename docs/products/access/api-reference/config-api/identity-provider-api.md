# Identity Providers

These APIs allow the retrieval of configuration of [identity-providers](../../topics/general-app-config/identity-providers/identity-providers.md) via a REST
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

| Param          | Required | Description                                                                  
|----------------|----------|------------------------------------------------------------------------------
| idpId          | yes      | Unique identifier of the Identity Provider.

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

## Error codes

One of the following responses will be returned, containing a JSON object with an error code, a message and details about the error.

| HTTP status | Error code                       | Message 
|-------------|----------------------------------|-------------------------------------------------------------------------------------
| 400         | invalid_request                  | One or more parameters is missing or incorrect. The details contain the missing or incorrect parameters.
| 401         | unauthorized                     | Provide valid credentials to get access to the API.
| 403         | forbidden                        | Operation is not allowed for the current user. 
| 404         | not_found                        | Identity Provider for given ID cannot be found
