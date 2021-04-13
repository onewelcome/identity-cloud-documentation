# REST Scope Verification Service API specification

This section describes the API that Onegini Access expects when the REST Scope Verification Service is used.

See the topic guide about [Scope Verification Service](../../topics/integration-extension/scope-verification/scope-verification.md) for instructions on enabling
this REST service.

The Scope Verification Service endpoint accepts POST requests with `application/json` or `application/json;charset=UTF-8` as the content-type header. Optionally
it can be protected with basic authentication. The request body is a json object containing the following properties:

| Property        | Example                                                               | Description
|-----------------|-----------------------------------------------------------------------|--------------------------------------------
| account_id      | `MyAccount`                                                           | Unique identifier of the account
| user_id         | `xxxyyy`                                                              | User identifier as issued by the (external) identity provider
| scopes          | `[{"id": "read", "service_endpoint": "https://service.example.com"}]` | An array containing scopes to be verified

The endpoint is expected to return `200 OK` status code along with json object with the following properties:

| Property             | Example      | Description                                                                                                 |
|----------------------|--------------|-------------------------------------------------------------------------------------------------------------|
| verification_result  | `SUCCESS`    | possible values are `SUCCESS` or `FAILURE`                                                                  |
| unauthorized_scope   | `read`       | only present when value of `verification_result` is `FAILURE`, contains scope for which verification failed |

## Example request

```http
POST /verify-scope  HTTP/1.1
Host: service.example.com
Content-Type: application/json;charset=UTF-8
Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==

{
  "account_id": "MyAccount",
  "user_id": "xxxyyy",
  "scopes": [
      {"id": "read", "service_endpoint": "https://readservice.example.com"}, 
      {"id": "write", "service_endpoint": "https://writeservice.example.com"}
  ]
}
```

## Example response

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8

{
  "verification_result": "FAILURE",
  "unauthorized_scope": "read"
}
```
