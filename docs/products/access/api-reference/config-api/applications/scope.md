# Scopes API

This allows the creation of new Scopes via a REST API. It can be utilized in scripts to add many scopes at once, edit or delete them.

All endpoints are protected with API client using either Client Secret Basic or PrivateKeyJWT [authentication method](../../topics/authentication-methods/authentication-methods.md). It requires an API client with the scope `onegini_api_config` (Config API).

## Endpoints

### Read Scope

This returns configuration of a given Scope.

* Endpoint: `/api/v1/configuration/scopes/{scope}`
* Method: GET

Path parameters:

| Param | Required | Description
|-------|----------|--------------------------------
| scope | yes      | Unique identifier of the Scope. 

Example request:

```http
GET /api/v1/configuration/scopes/openid HTTP/1.1
Host: onegini.example.com
```

Example success response:

```http
HTTP/1.1 200 Ok
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
{
  "scope_id": "openid",
  "authentication_level": 0,
  "usage_limit": 0,
  "service_endpoint": "",
  "persistent_consent": true,
  "descriptions": {
    "de": "openid",
    "en": "openid",
    "fr": "openid",
    "nl": "openid"
  }
}
```

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Create Scope

This creates a Scope from scratch

* Endpoint: `/api/v1/configuration/scopes`
* Method: POST

JSON body parameters:

| Param                        | Required | Example                       | Description
|------------------------------|----------|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| scope_id                     | yes      | "openid"                      |  Scope identifier. At most 20 characters: letters (a-z, A-Z), digits (0-9), underscores (_) and hyphens (-) permitted.
| authentication_level         | no       | 0                             |  Aauthentication level. If not provided defaults to 0. The Required authentication level of the user. If the Authentication level doesn't match, the user will be redirected to the configured authentication server.
| usage_limit                  | no       | 0                             |  Usage limit. If not provided defaults to 0. The number of times an Access token for this Scope can be used. When the value is 0, the Access token usage is unlimited.
| service_endpoint             | no       | "https://onegini.example.com" |  Service endpoint. If not provided defaults to null.
| verification_failed_endpoint | no       | "https://onegini.example.com" |  Verification failed endpoint. If not provided defaults to null.
| persistent_consent           | no       | true                          |  Persistent consent. If not provided defaults to false. When enabled the user has to give Consent for this Scope during only the first Authorization request. When disabled the User has to give Consent for every Authorization request with this Scope.
| descriptions                 | no       | { "en": "openid" }            |  Descriptions. If not provided, no descriptions will be stored.

Example request:

```http
POST /api/v1/configuration/scopes HTTP/1.1
Host: onegini.example.com
Content-Type: application/json
{
  "scope_id": "insurance"
}
```

Example success response:

```http
HTTP/1.1 201 CREATED
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
Location: /api/v1/configuration/scopes/insurance
```

The success response body is empty.

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Update Scope

Some fields can be updated after creating a Scope

* Endpoint: `/api/v1/configuration/scopes/{scope}`
* Method: PATCH

Path parameters:

| Param | Required | Description
|-------|----------|---------------------------------
| scope | yes      | Unique identifier of the Scope.


JSON body parameters:

Fields that are sent in the request will be changed to new value. Fields that are not sent will be reset to default values.

| Param                        | Required | Example                       | Description
|------------------------------|----------|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| scope_id                     | yes      | "openid"                      |  Scope identifier. At most 20 characters: letters (a-z, A-Z), digits (0-9), underscores (_) and hyphens (-) permitted.
| authentication_level         | no       | 0                             |  Aauthentication level. If not provided defaults to 0. The Required authentication level of the user. If the Authentication level doesn't match, the user will be redirected to the configured authentication server.
| usage_limit                  | no       | 0                             |  Usage limit. If not provided defaults to 0. The number of times an Access token for this Scope can be used. When the value is 0, the Access token usage is unlimited.
| service_endpoint             | no       | "https://onegini.example.com" |  Service endpoint. If not provided defaults to null.
| verification_failed_endpoint | no       | "https://onegini.example.com" |  Verification failed endpoint. If not provided defaults to null.
| persistent_consent           | no       | true                          |  Persistent consent. If not provided defaults to false. When enabled the user has to give Consent for this Scope during only the first Authorization request. When disabled the User has to give Consent for every Authorization request with this Scope.
| descriptions                 | no       | { "en": "openid" }            |  Descriptions. If not provided, no descriptions will be stored.

Example request:

```http
PATCH /api/v1/configuration/scopes/insurance HTTP/1.1
Host: onegini.example.com
Content-Type: application/json

{
  "scope_id": "insurance",
  "descriptions": {
    "nl": "verzekering"
  }
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

### Delete Scope

This removes a Scope.

* Endpoint: `/api/v1/configuration/scopes/{scope}`
* Method: DELETE

Path parameters:

| Param | Required | Description
|-------|----------|--------------------------------
| scope | yes      | Unique identifier of the Scope.


Example request:

```http
DELETE /api/v1/configuration/scopes/insurance HTTP/1.1
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
|-------------|----------------------------------|---------------------------------------------------------------------------------------------------------
| 400         | invalid_request                  | One or more parameters is missing or incorrect. The details contain the missing or incorrect parameters.
| 401         | unauthorized                     | Provide valid credentials to get access to the API.
| 403         | forbidden                        | Operation is not allowed for the current user.
| 404         | not_found                        | Scope configuration cannot be found 
| 409         | conflict                         | The Scope already exists
