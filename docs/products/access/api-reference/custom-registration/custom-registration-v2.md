# Custom Registration v2

## Authentication

This endpoint requires the client to provide the Client Authentication in the form of a PrivateKeyJWT
(`urn:ietf:params:oauth:client-assertion-type:jwt-bearer`) assertion, for more info
read [authentication methods](../../topics/authentication-methods/authentication-methods.md).

This API has specific demands for the PrivateKeyJWT authentication:

* The JWT must be signed with algorithm `ES256`.
* Its key identifier must be present as `kid` in the header of the JWT.
* The signature is verified. Configure the public key or the JWKs endpoint in
  the [client configuration](../../topics/web-clients/web-client-configuration.md).

## Init Step

The Init Step is only used for the `TWO_STEP` flow. The `ONE_STEP` flow uses the Complete Step.

Endpoint: `POST /oauth/v2/custom-registration/{idp}/init`

| Parameter | Description                  |
|-----------|------------------------------|
| `idp`     | Identity provider identifier |

JSON body parameters:

| Param                   | Required | Description                                                                                                                                                  |
|-------------------------|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `client_assertion_type` | yes      | The type of the client assertion that is part of the request, currently the only supported value is `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| `client_assertion`      | yes      | Client assertion in the form of a JWT signed by the client's private key                                                                                     |
| `data`                  | no       | Raw registration request data which will be provided to the Extension Engine                                                                                 |

Example request:

```http
POST /oauth/v2/custom-registration/example-custom-registration-idp/init HTTP/1.1
Host: onegini.example.com
Content-Type: application/json

{
   "client_assertion_type" : "urn:ietf:params:oauth:client-assertion-type:jwt-bearer",
   "client_assertion" : "eyJhbGciOiJSUzI1NiIsImtpZCI6IjIyIn0.
     eyJpc3Mi[...omitted for brevity...].
     cC4hiUPo[...omitted for brevity...]",
  "data" : "{\"custom_json_key\":\"custom json data\"}" // optional
 }
```

Example success response:

```http
HTTP/1.1 200 Ok
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache

{
   "transaction_id": "123123",   //something unique, should be passed with complete step
   "data": "12349876",           // e.g. a challenge code.
   "status": 2000
}
```

In the event of an error in the Access Service, one of the following [error codes](#server-error-codes) will be returned.

It is up to the scripts executed by the Extension Engine to determine if the request was successful or not when everything looks fine for
the Access Service. For all these scenarios, a 200 OK JSON response returned to the SDK which contains:

| Param            | Description                                                                                                          |
|------------------|----------------------------------------------------------------------------------------------------------------------|
| `transaction_id` | Generated in the **Init step**. For TWO_STEP, this transaction identifier ensures that the same transaction is used. |
| `data`           | Raw response coming from the script engine.                                                                          |
| `status`         | Status indicating whether the request was successful. See [status codes](#extension-engine-status-codes).            |

## Complete Step

Endpoint: `POST /oauth/v2/custom-registration/{idp}/complete`

| Parameter | Description                  |
|-----------|------------------------------|
| `idp`     | Identity provider identifier |

JSON body parameters:

| Param                   | Required                                | Description                                                                                                                                                  |
|-------------------------|-----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `client_assertion_type` | yes                                     | The type of the client assertion that is part of the request, currently the only supported value is `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| `client_assertion`      | yes                                     | Client assertion in the form of a JWT signed by the client's private key                                                                                     |
| `transaction_id`        | yes (`TWO_STEP`) otherwise optional     | Generated in **Init step**. For `TWO_STEP`, ensures same transaction                                                                                         |
| `data`                  | no                                      | Raw registration request data which will be provided to the Extension Engine                                                                                 |
| `scope`                 | no                                      | An array of scopes. If none are specified the default scopes are granted.                                                                                    |

Example request:

```http
POST /oauth/v2/custom-registration/example-custom-registration-idp/complete HTTP/1.1
Host: onegini.example.com
Content-Type: application/json

{
   "client_assertion_type" : "urn:ietf:params:oauth:client-assertion-type:jwt-bearer",
   "client_assertion" : "eyJhbGciOiJSUzI1NiIsImtpZCI6IjIyIn0.
     eyJpc3Mi[...omitted for brevity...].
     cC4hiUPo[...omitted for brevity...]",
  "transaction_id": "123123",
  "data": "{\"custom_json_key\":\"custom_ json data\"}",      //optional, e.g. challenge code response
  "scope": ["read", "write"],
}
```

Example successful response

```http
HTTP/1.1 200 Ok
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache

{
    "status": 2000,
    "oauth_token": {
        "token_type": "bearer",
        "access_token": "8A5AB83A3C6B7AAC41471C1205167A35E0F9281ED277EE2FDE6E8DE30972936D",
        "refresh_token": "8CAE26B2B8E8EC18B4D432886448C7F99B558063C517BA41F30966B37C104983",
        "id_token": "eyJraWQiOiI1Nzk1[...omitted for brevity...]r9KM8c5y-Utpw",
        "expires_in": 3600
    },
    "data": "{\"custom_json_key\":\"custom json data\"}", // optional
}
```

In the event of an error in the Access Service, one of the following error codes will be returned:

## Server error codes

| Status code | Error code             | Description                                                                                                                   |
|-------------|------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| 400         | invalid_request        | Missing required parameter or the request is not correctly formatted.                                                         |
| 400         | invalid_request        | Client authentication missing or not supported                                                                                |
| 400         | invalid_client         | Client authentication failed (e.g., unknown client, no client authentication included, or unsupported authentication method). |
| 404         | invalid_idp_identifier | The specified IdP does not exist.                                                                                             |
| 403         | idp_disabled           | The specified IdP is disabled.                                                                                                |
| 400         | invalid_client         | Not a valid client.                                                                                                           |
| 400         | invalid_transaction    | The transaction is invalid or has expired                                                                                     |
| 400         | invalid_scope          | The requested scope is invalid, unknown or malformed.                                                                         |

It is up to the scripts execution in the Extension Engine to determine if the request was successful or not when everything looks fine for
the Access Service. For all these scenarios, a `200 OK` JSON response is returned to the SDK which contains:

| Param                     | Description                                                                                               |
|---------------------------|-----------------------------------------------------------------------------------------------------------|
| `access_token`            | Access token generated after successful completion of step.                                               |
| `refresh_token`           | Refresh token generated after successful completion of step and client has them enabled.                  |
| `expires_in`              | Time until expiration in seconds.                                                                         |
| `token_type`              | Token type.                                                                                               |
| `id_token`                | ID token with user data if the requested scope contains `openid`.                                         |
| `data`                    | Raw response coming from the script engine.                                                               |
| `status`                  | Status indicating whether the request was successful. See [status codes](#extension-engine-status-codes). |

## Extension engine status codes

| Param                        | Value  |
|------------------------------|--------|
| `VALID_STATUS_MIN`           | 2000   |
| `VALID_STATUS_MAX`           | 2999   |
| `RETRY_STATUS_MIN`           | 4000   |
| `RETRY_STATUS_MAX`           | 4999   |
| `UNRECOVERABLE_STATUS_MIN`   | 5000   |
| `UNRECOVERABLE_STATUS_MAX`   | 5999   |
