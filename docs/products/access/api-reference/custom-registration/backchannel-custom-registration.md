# Custom Registration Backchannel Communication API

### Backchannel Communication

With a [custom API Identity Provider](../../topics/custom-registration/index.md), you may wish to execute some logic or store some data that
will be utilized
during user registration via a server that is outside the scope of a mobile application. You can use this endpoint to execute a custom
script within the
Onegini Extension Engine to store data to be used in later steps.

See the [Example Scripts](../../topics/custom-registration/custom-api-example.md) for help creating the scripts.

## Execute the script for a specific identity provider

Endpoint: `POST /oauth/api/v3/custom-registration/{idp}/backchannel`

| Parameter | Description                  |
|-----------|------------------------------|
| `idp`     | Identity provider identifier |

JSON body parameters:

| Param   | Required  | Description                                                                          |
|---------|-----------|--------------------------------------------------------------------------------------|
| `data`  | no        | Raw registration request data which will be provided to the Onegini Extension Engine |

This endpoint requires valid API client credentials provided via basic authentication or via url encoded form.
It requires the API Client have the `User Registration` scope.

Example request using `ExampleId` for the identity provider id:

```http
POST /oauth/api/v3/custom-registration/ExampleId/backchannel HTTP/1.1
Host: onegini.example.com
Content-Type: application/json

{
   "data": "{\"custom_key\":\"custom_data\"}" //optional
}
```

Example success response:

```http
HTTP/1.1 200 Ok
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache

{
   "status": 2000,
   "data": "{\"custom_response_key\":\"custom_response_data\"}"
}
```

## Access Error Codes

One of the following responses will be returned, containing a JSON object with an error code.

| Status code | Error code               | Description                                                         |
|-------------|--------------------------|---------------------------------------------------------------------|
| 400         | invalid_client           | Client credentials used are not valid or client does not exist      |
| 403         | insufficient_permissions | The API client does not have the required `User registration` scope |
| 403         | idp_disabled             | The specified IdP is disabled.                                      |
| 404         | invalid_idp_identifier   | The specified IdP does not exist.                                   |
| 500         | internal_server_error    | The server has encountered an internal server error.                |


