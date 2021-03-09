# Mobile Authentication API version 3

This section describes the API for mobile authentication, version 3. The latest version of the Mobile Authentication API
is [version 4](./mobile-authentication-v4.md).

## New in this API version

* A user can enroll for mobile push authentication for multiple applications at the same time.

## API access

> **Note**: To access the API, mobile authentication has to be enabled on [system level](../../topics/technical-app-management/system-features-config/system-features-config.md).

Only authorized API clients can access this API. Its credentials need to be provided via HTTP Basic Authentication header or via url encoded form. The required
credentials are the client id and client secret configured in the Admin console of Onegini Access.
> **Note:** Please look in the [OAuth config](../../appendix/administration/api-config.md) section on how to configure an API client.
> In order to allow an API client to access mobile authentication endpoints add `Mobile authentication` to the `Valid for APIs` list in the API client configuration.

In case the provided client credentials are invalid or the client is not authorized to access the API, the errors according to
[OAuth specification Error Response](https://tools.ietf.org/html/rfc6749#section-5.2) will be returned.

Example request with invalid Basic Authentication header (invalid client id and client secret):

```http
POST /oauth/api/v3/authenticate/user HTTP/1.1
Host: onegini.example.com
Authorization: Basic aW52YWxpZDppbnZhbGlk
Content-Type: application/x-www-form-urlencoded

 
callback_uri=https%3A%2F%2Fwww.myportal.com%2Fcallback&message=Please%20authenticate%20for%20mine.acme.com&type=push_with_pin&user_id=myUserId&app_id=myApp
```

Example failure result

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json;charset=UTF-8
Cache-Control: no-cache, no-store, must-revalidate
Pragma: no-cache
 
{
    "error": "invalid_client",
    "error_description": "Client authentication failed (e.g., unknown client, no client authentication included, or unsupported authentication method)."
}
```

## Availability

Onegini Access provides two endpoints to check the availability of mobile authentication types for specific users.

### Check if authentication is available for user and application

Endpoint: `GET /oauth/api/v3/authenticate/user/{user_id}/application/{app_id}/enabled`

| Parameter   | Description                                                                                      |
|-------------|--------------------------------------------------------------------------------------------------|
| `user_id`   | Identifier of the user                                                                           |
| `app_id`    | Identifier of the (mobile) app that handles the mobile authentication                            |

In all scenarios a `200 OK` response is returned by the server if mobile authentication is enabled on system level. The response contains a JSON message with a
list of available authentication types.

When there are no authentication options for a user/application pair or the user/application is unknown, the endpoint will return an empty list.

> **Note:** A mobile authentication type is available when either its original method or its fallback method is available to authenticate with.

**Example response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-cache, no-store, must-revalidate
Pragma: no-cache

{
  "enabled":[
    "login_with_push",
    "login_with_pin",
    "authorize_with_pin", 
    "sms"
  ]
}
```

### Check available authentication options for user

Endpoint: `GET /oauth/api/v3/authenticate/user/{user_id}/enabled`

| Parameter | Description                                                                                     |
|-----------|-------------------------------------------------------------------------------------------------|
| `user_id` | Identifier of the user                                                                          |

In all scenarios a `200 OK` response is returned by the server if mobile authentication is enabled on system level. The response contains a JSON message with a
list of available authentication options for a user. Each option contains the mobile authentication types as defined via the admin console. Only the
applications for which the user has enrolled push authentication can handle this method. These applications are listed in `apps_enrolled_for_push`. Mobile
authentication that is based on SMS or OTP is not restricted to specific applications. Push based authentication types can have sms fallback configured. This is
also reflected in the JSON with the `sms_fallback_allowed` attribute.

When there are no authentication options for a user or the user is unknown the endpoint will return an empty list.

> **Note:** A mobile authentication type is available only if its original method or its fallback method is available to authenticate with.

**Example response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-cache, no-store, must-revalidate
Pragma: no-cache

{
  "enabled": [
    {
      "type": "authorize_with_push",
      "method": "PUSH",
      "sms_fallback_allowed": true,
      "apps_enrolled_for_push": [
        {
          "id": "myApp",
          "name": "My application"
        },
        {
          "id": "myOtherApp",
          "name": "My other application"
        }
      ]
    },
    {
      "type": "authorize_with_pin",
      "method": "PUSH_WITH_PIN",
      "sms_fallback_allowed": false,
      "apps_enrolled_for_push": [
        {
          "id": "myApp",
          "name": "My application"
        },
        {
          "id": "myOtherApp",
          "name": "My other application"
        }
      ]
    },
    {
      "type": "authorize_with_fingerprint",
      "method": "PUSH_WITH_FINGERPRINT",
      "sms_fallback_allowed": false,
      "apps_enrolled_for_push": [
        {
          "id": "myApp",
          "name": "My application"
        },
        {
          "id": "myOtherApp",
          "name": "My other application"
        }
      ]
    },
    {
      "type": "authorize_with_otp",
      "method": "OTP"
    },
    {
      "type": "authorize_with_sms",
      "method": "SMS"
    }
  ]
}
```

## Mobile authentication initialization

Mobile authentication can be initialized by an asynchronous API call to Onegini Access. For most mobile authentication types (push, OTP) the portal will be
informed of the authentication result via a callback.

#### Request

Endpoint: `POST /oauth/api/v3/authenticate/user`

The request must contain a body which is `application/x-www-form-urlencoded;charset=UTF-8` encoded. The parameters required depend on the type of authentication
requested. Note that if a fallback is used for the mobile authentication type, and this fallback is of a different type, then its required parameters should
also be present.

| Parameter      | Req'd PUSH | Req'd OTP | Req'd SMS | Description                                                                                                                                                                                                                                                                                                                                                                                                                                |
|----------------|------------|-----------|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type           | Yes        | Yes       | Yes       | The name of the mobile authentication type as defined via admin panel. Example: no_pin, pin, etc. Each push based mobile authentication type may contain a fallback strategy which is used when the original authentication method is not available for a user.                                                                                                                                                                                                                          |
| user_id        | Yes        | No        | Yes       | Identifier of the user. Must NOT be present for OTP.                                                                                                                                                                                                                                                                                                                                                                                       |
| app_id         | Yes        | No        | No        | Identifier of the application that handles mobile authentication.                                                                                                                                                                                                                                                                                                                                                                                       |
| callback_uri   | Yes        | Yes       | No        | Uri on which the portal wants to receive the result of the authentication. When a callback uri whitelist is used, the callback uri should be on the whitelist.                                                                                                                                                                                                                                                                             |
| message        | Yes        | Yes       | Yes       | The message to display in the authentication request. This message may ONLY be omitted if default messages have been specified for the mobile authentication type. NOTE: Max allowed length is 155 by default.                                                                                                                                                                                                                             |
| signing_data   | No         | No        | No        | The data which will be signed if the mobile authentication type has transaction signing enabled (only available for push). This data will be delivered to the device in encrypted format. If transaction signing is not enabled for the type, this field will be ignored.                                                                                                                                                                  |
| secure_message | No         | No        | No        | Sensitive message which will be encrypted and delivered to the device with HTTPS                                                                                                                                                                                                                                                                                                                                                           |
| phone_number   | No         | No        | Yes       | Phone number to be used in SMS authentication process.                                                                                                                                                                                                                                                                                                                                                                                     |
| language_code  | No         | No        | No        | When no message is specified, the language code is used to determine which message to use from the configured default messages. When no default message is found for the language code the English message is used. Examples of language codes: nl, fr, en, de (lowercase).  |

**Example push request:**

```http
POST /oauth/api/v3/authenticate/user HTTP/1.1
Host: onegini.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded
 
user_id=myUserId&callback_uri=https%3A%2F%2Fwww.myportal.com%2Fcallback&message=Please%20authenticate%20for%20mine.acme.com&type=push_with_pin&app_id=myApp
```

**Example OTP request:**

```http
POST /oauth/api/v3/authenticate/user HTTP/1.1
Host: onegini.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded
 
type=otp&callback_uri=https%3A%2F%2Fwww.myportal.com%2Fcallback&message=Please%20authenticate%20for%20mine.acme.com
```

#### Response

When the mobile authentication server was able to successfully initialize the mobile authentication request, a `200 OK` response is received containing several
parameters, depending on the type of authentication initialized.

| Parameter      | PUSH | OTP | SMS | Description
|----------------|------|-----|-----|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| transaction_id | Yes  | Yes | Yes | A UUID that can be used to fetch the mobile authentication result, for example after having received a callback.
| auth_method    | Yes  | Yes | Yes | A description of which method was used. This information can be useful in case the fallback strategy was triggered and the authentication method is different than the expected one.
| time_to_live   | Yes  | Yes | Yes | The time to live of the mobile authentication request, in milliseconds. After expiry, an unanswered mobile authentication request is considered closed and unsuccessful.
| otp            | No   | Yes | No  | The OTP code that the portal can deliver to the SDK via some method (for example a QR code). This code is then used to authenticate. Care should be taken not to leak this code to other parties.
| device         | Yes  | No  | No  | The device object contains details about the device the mobile authentication request was sent to. The `name` contains the device name that the user gave to his/her device. The `platform` is the device platform and can either have the values `ios` or `android`.

**Example push success result:**

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-cache, no-store, must-revalidate
Pragma: no-cache
 
{
  "transaction_id": "5927555c-d40c-408d-8149-b3db671e14cc",
  "auth_method": "push",
  "time_to_live": 60000,
  "device": {
    "name": "Lisa's Android device",
    "platform": "android"
  }
}
```

When the mobile authentication server was able to successfully send out an authentication request to the user a 200 OK response is received containing a
transaction id which is an UUID. Based on this transaction id the portal can correlate the incoming authentication results. The returned object also contains an
information about authentication method which was actually used (PUSH or SMS) plus the time to live for the transaction (in milliseconds). Information may be
useful in situations where for some reason PUSH authentication failed, but in the initial request portal also provided user's phone number so Onegini Access was
able to fallback on the SMS authentication method.

**Example failure result:**

```http
HTTP/1.1 404 NOT FOUND
Content-Type: application/json;charset=UTF-8
Cache-Control: no-cache, no-store, must-revalidate
Pragma: no-cache

{
  "error": "not_found",
  "error_description": "No authentication possibilities for user/application or user not found."
}
```

## Mobile authentication result

Once the user has completed authentication, the portal must be informed of the results. This is done via a callback from Onegini Access to the portal, and a
subsequent request to fetch the mobile authentication results.

**Note:** This section does not apply to SMS mobile authentication types. Please refer to the SMS endpoints section for completing an SMS mobile authentication
request.

### Callback

Once authentication has occurred (or failed), Onegini Access calls the endpoint provided by the portal during the mobile authentication initialization step (
specifically, the `callback_uri` field).

Endpoint: `POST <callback_uri>`

**Example request:**

```http
POST /callback HTTP/1.1
Host: www.myportal.com
Content-Type: application/json;charset=UTF-8

{
  "callback_uri":"https://www.myportal.com/callback",
  "transaction_id":"cf51a27a-df80-42ad-bdd7-936c476b2c87",
}
```

| Parameter        | Description                                                                                 |
|------------------|---------------------------------------------------------------------------------------------|
| `callback_uri`   | The callback uri used in the initialization request and where this callback is sent to.     |
| `transaction_id` | The identifier of the authentication transaction (received in the initialization response). |

The portal should send a "204 No Content" response on the callback.

### Fetch authentication result

When the callback has been received by the portal, this means that the mobile authentication request has been completed and the results can be retrieved.

Endpoint `GET /oauth/api/v3/authenticate/transaction/{transaction_id}`

| Parameter        | Description                                                 |
|------------------|-------------------------------------------------------------|
| `transaction_id` | transaction identifier returned in authentication init step |

**Example request:**

```http
GET oauth/api/v3/authenticate/transaction/cf51a27a-df80-42ad-bdd7-936c476b2c87 HTTP/1.1
Host: onegini.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded
```

The response will contain a JSON object with the following parameters:

| Parameter                      | Description                                                                                                                                                                                                                                       |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `callback_uri`                 | The callback uri used in the authentication request and where the callback was sent to.                                                                                                                                                           |
| `transaction_id`               | The identifier of the mobile authentication transaction.                                                                                                                                                                                          |
| `timestamp`                    | Timestamp of the moment the authentication request was sent.                                                                                                                                                                                      |
| `user_id`                      | The identifier of the user that has answered the authentication request.                                                                                                                                                                          |
| `is_authenticated`             | The authentication result. Equal to `true` if the user has been authenticated or `false` otherwise.                                                                                                                                               |
| `authentication_method`        | The authentication method used to answer the authentication request. This can be useful in the event of a fallback.                                                                                                                               |
| `used_authentication_attempts` | Number of attempts used to authenticate with the method (only provided for `PUSH_WITH_PIN`).                                                                                                                                                      |
| `signature`                    | The cryptographic signature of the `signing_data` (data that needed to be signed by the user, provided on initialization request by the portal). Only included for transaction signing.                                                           |
| `user_public_key`              | The public key used to sign the `signing_data` (data that needed to be signed by the user, provided on initialization request by the portal). The public key is PEM encoded (e.g. base64 with linebreaks). Only included for transaction signing. |
| `signature_verified`           | The result of the signature. True when the signature was verified using the user's public key, and false when the signature was unable to be verified. Only included for transaction signing.                                                     |                                                      
| `not_authenticated_reason`     | The reason of not authenticating the user. Only included when the user failed to become authenticated with no possibility for another try (after callback was sent). It contains `reason` and `description` labels (see the not authenticated example response below)  |

The following table shows possible not authenticated reasons:

| `reason`          | `description`                       |
|-------------------|-------------------------------------|
| failed_to_send    | Push message failed to be sent      |
| invalid_request   | Invalid push authentication request |
| not_accepted      | User rejected push                  |
| invalid_answer    | Invalid push answer                 |

**Example authenticated response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-cache, no-store, must-revalidate
Pragma: no-cache
 
{
  "callback_uri":"https://www.myportal.com/callback",
  "transaction_id":"cf51a27a-df80-42ad-bdd7-936c476b2c87",
  "timestamp":1402390451564,
  "user_id":"myDummyUser",
  "is_authenticated":true,
  "authentication_method": "push_with_pin",
  "used_authentication_attempts": 1,
  "signature": "CpjXBItoNacFfr1OmqQMW4phDinOulptu9OizQVKnqm49JPoUlg=",
  "user_public_key": "-----BEGIN PUBLIC KEY-----
    MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAt2SQBWzrMN8BEI2Q7Bv1
    cHif/Zq64XvcHRQ+Rh9oQRftBeWTKKAHEZK4MFAB+Z/xypP+pzJz4qixxaOlOph9
    g210m6fS6l+DoX1Q3gb+rA5TEIDDtLGTO8nEJoclbf8c9B6E1nnPa5T8Mwpu1S3N
    otrix4dY5bkyI6/MX5amFvp3BN8eZWrzJvtmO8ERb7YROz96t/VQp7wczqnlx42U
    +D2WjNqWEmy7iuB33iqKp3cevlVY33ZS8oTMSc9nLhfh95so+Ioi2943WjDPNTgk
    Ftf36bIShB4IksF2F1nlszavZ2WTaIeiqCsoPg9Bx0d6y2HG4mA4nJhWlk2A9Jyu
    EwIDAQAB
    -----END PUBLIC KEY-----",
  "signature_verified":true
}
```

**Example not authenticated response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-cache, no-store, must-revalidate
Pragma: no-cache
 
{
  "callback_uri":"https://www.myportal.com/callback",
  "transaction_id":"cf51a27a-df80-42ad-bdd7-936c476b2c87",
  "timestamp":1402390451564,
  "user_id":"myDummyUser",
  "is_authenticated":false,
  "used_authentication_attempts": 1,
  "not_authenticated_reason":{
    "reason":"invalid_answer",
    "description":"Invalid push answer"
  }
}
```

In the event of an error, one of these [error codes](#error-codes) will be returned.

## SMS endpoints

While mobile authentication with SMS is initialized using the same endpoint as other types, the completion of the request differs in that the user must submit
their SMS code to the portal. Onegini Access therefore provides two endpoints for this purpose.

### Verify SMS code

The portal can verify a user's SMS code using the following endpoint.

Endpoint: `POST /oauth/api/v3/authenticate/user/{user_id}/sms`

| Parameter  | Description            |
|------------|------------------------|
| `user_id`  | Identifier of the user |

The request body must be `application/x-www-form-urlencoded;charset=UTF-8` encoded and contain the following parameters:

| Parameter        | Description                                                                          |
|------------------|--------------------------------------------------------------------------------------|
| `transaction_id` | Transaction identifier returned during the mobile authentication initialization step |
| `sms_code`       | verification code provided by user                                                   |

**Example request:**

```http
POST oauth/api/v3/authenticate/user/myUserId/sms HTTP/1.1
Host: onegini.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded
 
sms_code=123456&transaction_id=5927555c-d40c-408d-8149-b3db671e14cc
```

**Example success response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-cache, no-store, must-revalidate
Pragma: no-cache
 
{
  "transaction_id": "5927555c-d40c-408d-8149-b3db671e14cc"
}
```

**Example error response:**

```http
HTTP/1.1 400 Bad request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-cache, no-store, must-revalidate
Pragma: no-cache
 
{
  "error": "invalid_verification_code",
  "error_description": "The verification code is invalid."
}
```

### Resend verification SMS

The following endpoint can be used to resend an SMS verification code in case the previous one was not delivered.

Endpoint `POST /oauth/api/v3/authenticate/user/{user_id}/sms/resend`

| Parameter  | Description            |
|------------|------------------------|
| `user_id`  | Identifier of the user |

The request body must be `application/x-www-form-urlencoded;charset=UTF-8` encoded and contain the following parameters:

| Path variable    | Description                                                                             |
|------------------|-----------------------------------------------------------------------------------------|
| `transaction_id` | The identifier of the authentication transaction.                                       |

**Example request:**

```http
POST oauth/api/v3/authenticate/user/myUserId/sms/resend HTTP/1.1
Host: onegini.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded
 
transaction_id=5927555c-d40c-408d-8149-b3db671e14cc
```

A `204 No Content` is returned when the SMS was successfully resent.

## Error codes

| Error code | Http status code | Description                                                            |
|------------|------------------|------------------------------------------------------------------------|
| 3002       | 503              | Failed to initiate authentication, failed to send sms                  |
| 3004       | 404              | Failed to authenticate, invalid transaction id                         |
| 3005       | 404              | Failed to authenticate, invalid user id                                |
| 3006       | 403              | Resend limit reached.                                                  |

## Error codes

| Error code | Http status code | Description
|------------|------------------|------------------------------------------------------------------------
| 1000       | 404              | Mobile authentication disabled
| 1001       | 404              | No authentication possibilities for user or user not found
| 1002       | 503              | Failed to initiate authentication at authentication provider
| 1003       | 400              | One of the requests parameters is invalid or missing
| 1005       | 400              | Failed to initiate authentication, message content too long
| 1006       | 404              | Failed to fetch authentication message
| 3000       | 404              | SMS authentication disabled
| 3001       | 400              | Invalid input params, phone number is missing
| 3002       | 503              | Failed to initiate authentication, failed to send SMS
| 3003       | 404              | Failed to authenticate, verification code invalid
| 3004       | 404              | Failed to authenticate, invalid transaction id
| 3005       | 404              | Failed to initiate authentication, Mobile authentication type not found
| 4000       | 404              | Failed to initiate authentication, Failed to initiate authentication, authentication properties not found
| 4001       | 404              | Server configuration mandates use of API version 4 and above for mobile authentication