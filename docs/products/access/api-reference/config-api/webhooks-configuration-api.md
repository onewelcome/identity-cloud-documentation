# Web Hooks Configuration API

This allows the maintenance of the WebHook definitions that can be later used by the OAuth Clients. The WebHooks can serve different
purposes ranging from
making access decisions to user details manipulation.

All endpoints are protected with the API client credentials (either Client Secret Basic or PrivateKeyJWT depending on the client's
authentication method).
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
         "authentication_method":"JWT"
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

| Param                 | Required | Example                                       | Description                                                                                                                                                                                            |
|-----------------------|----------|-----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                    | no       | "c7b34d6a-682e-4eb2-8d1d-af2842108867"        | Unique identifier for the configuration                                                                                                                                                                |
| type                  | yes      | "DABP"                                        | The type of the WebHook, allowed values are `CUSTOMIZE_TOKEN`, `DABP` and `USER_DETAILS_CUSTOMIZATION`                                                                                                 |
| name                  | yes      | "Delegated Administration production cluster" | Human readable name of the configuration                                                                                                                                                               |
| base_uri              | yes      | "https://dabp-prod.example.com"               | Base URI that Access Service can reach for calls to Web Hook API                                                                                                                                       |
| authentication_method | yes      | "JWT"                                         | The type of the authentication required by the WebHook, allowed values are `JWT`, `BASIC`(deprecated) and `NONE`. More details on Authentication methods can be found [here](#authentication-methods). |
| username              | no       | "dabp_user"                                   | Username to call the Web Hook API. Required when authentication method is `BASIC`.                                                                                                                     |
| password              | no       | "AF33E2BF29C54A4639AB…"                       | Password (not returned on GET). Required when authentication method is `BASIC`.                                                                                                                        |

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

| Param | Required | Description                                     |
|-------|----------|-------------------------------------------------|
| id    | yes      | Unique identifier of the Web Hook configuration |

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
   "authentication_method":"JWT",
   "username": "dabp_user"
} 
```

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Update Web Hook configuration

Some fields can be updated after creating a Web Hook configuration.

* Endpoint: `/api/v1/configuration/web-hooks/{id}`
* Method: PATCH

Path parameters:

| Param | Required | Description                                      |
|-------|----------|--------------------------------------------------|
| id    | yes      | Unique identifier of the Web Hook configuration. |

JSON body parameters:

Only the fields that are sent in the request will be changed.

| Param                 | Required | Example                                       | Description                                                                                                       |
|-----------------------|----------|-----------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| name                  | no       | "Delegated Administration production cluster" | Human readable name of the configuration                                                                          |
| type                  | no       | "DABP"                                        | The type of the WebHook, allowed values are `CUSTOMIZE_TOKEN`, `DABP` and `USER_DETAILS_CUSTOMIZATION`            |
| base_uri              | no       | "https://dabp-prod.example.com"               | Base URI that Access Service can reach for calls to DUM engine                                                    |
| authentication_method | no       | "JWT"                                         | The type of the authentication required by the WebHook, allowed values are `JWT`, `BASIC`(deprecated) and `NONE`. |
| username              | no       | "dabp_user"                                   | Username to call the DUM engine APIs                                                                              |
| password              | no       | "F167433E63CE8BD874D7…"                       | Password (not returned on GET)                                                                                    |

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

This removes a Web Hook configuration. This will also remove any link to this configuration, e.g. from the web client or application
configuration.

* Endpoint: `/api/v1/configuration/web-hooks/{id}`
* Method: DELETE

Path parameters:

| Param | Required | Description                                     |
|-------|----------|-------------------------------------------------|
| id    | yes      | Unique identifier of the Web Hook configuration |

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

| HTTP status | Error code      | Message                                                                                                  |
|-------------|-----------------|----------------------------------------------------------------------------------------------------------|
| 400         | invalid_request | One or more parameters is missing or incorrect. The details contain the missing or incorrect parameters. |
| 401         | unauthorized    | Provide valid credentials to get access to the API.                                                      |
| 403         | forbidden       | Operation is not allowed for the current user.                                                           |
| 404         | not_found       | Web Hook configuration cannot be found for this id                                                       |
| 409         | conflict        | The id already exists for a different Web Hook configuration                                             |

# Authentication Methods

## JWT

JSON Web Tokens (JWT) is the preferred method for authentication.
JWT is a reliable, secure method of representing claims to be transferred between two parties.

Example JWT that Access generates for use in WebHooks:

```json
{
  "aud": "http://wiremock:8080",
  "sub": "onewelcomeAccessWebHookClient",
  "scope": [
    "onewelcome_webhooks",
    "onewelcome_webhook_test"
  ],
  "iss": "https://playground.test.onewelcome.io/oauth",
  "exp": 1705585175,
  "jti": "AT.4027b2b3-b3f8-4ff9-bef7-c7a0f01d6bec",
  "cid": "onewelcomeAccessWebHookClient"
}
```
- `aud` (Audience): Specifies the recipients that the JWT is intended for. It will contain `base_uri` from WebHook configuration
- `sub` (Subject): Identifies the principal that is the subject of the JWT. In this case, it represents the identity of the webhook client
  named `onewelcomeAccessWebHookClient`.
- `scope`: Defines the permission scope granted to the WebHook client. It will contain two scopes a fixed one `onewelcome_webhooks`
  and one based on the name of the WebHook - `onewelcome_webhook_<name>` where `<name>` will be replaced with lower cased WebHook name
  where space characters are replaced with underscore sign. For example, WebHook named `My WebHook` will contain
  scope `onwewelcome_webhook_my_webhook`.
- `iss` (Issuer): Identifies the principal that issued the JWT. It will contain URL of Access service which calls the WebHook.
- `exp` (Expiration Time): Specifies the expiration time after which the JWT must not be accepted for processing. It is a numeric value
  representing the number of seconds from 1970-01-01T00:00:00Z UTC until the specified UTC date/time.
  
  > Issued tokens are valid for 15 minutes and can be cached and reused by us for performance reasons.  
- `jti` (JWT ID): Provides a unique identifier for the JWT, which can be used to prevent the JWT from being replayed. It is a case-sensitive
  string.
- `cid` (Client ID): Identifies the client that requested the JWT. It will contain a fixed value `onewelcomeAccessWebHookClient`.

Token will be signed by Access Service and can be verified by using public keys available
at: `<value_of_iss_claim>/v1/keys` for example `https://customer.onewelcome.io/oauth/v1/keys`

## Basic Authentication (deprecated)

> **Note:** While old integrations built on Basic Authentication still function, we highly recommend updating them to use the JWT (JSON Web
> Tokens) method as soon as possible.

Basic Authentication is a simple authentication scheme built into the HTTP protocol. The client sends a base64 encoded string that contains
a username and a password separated by a colon `:`. This string is added to the header of the HTTP request in the `Authorization` field
with `Basic` as a type. For example:

`Authorization: Basic encoded_username_and_password`

The server then decodes the base64 string and authenticates the request based on these credentials.

Despite its simplicity, Basic Authentication is seen as insecure for most web applications because the username and password are sent across
the network in plaintext (albeit base64 encoded) and could be easily intercepted. It is vulnerable to man-in-the-middle attacks, and for
this reason, we favor safer alternative - JWT.

## No Authentication

No Authentication means there are no special encoding, keys, or credentials needed to access the API or webhook. Any API calls made do not
require any token or Basic Auth.

No Authentication is often used in public APIs or public resources where restricting access is not necessary.

However, while it's the easiest to implement, No Authentication provides no security as it allows anyone to access and interact with your
API or webhook. It is not recommended for any operations involving sensitive data, and should only be used sparingly, with a full
understanding of its implications.
