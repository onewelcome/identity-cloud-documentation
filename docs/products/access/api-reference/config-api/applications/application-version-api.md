# Application version API


This allows the configuration of [versions for mobile apps](../../../topics/mobile-apps/app-configuration/app-version-management.md) via a REST API. 
The Application version API offers the same functionality as the Admin interface and can be integrated in a Continuous Delivery (CD) environment.

## Endpoints

All endpoints are protected with the API client credentials (either client secret basic or private key JWT depending on the client [authentication method](../../../topics/authentication-methods/authentication-methods.md). It requires an API client with the scope `onegini_api_config` (Config API).

### Get a specific Application version

This returns the details of a specific Application version.

* Endpoint: `/api/v1/configuration/applications/{appId}/platforms/{platform}/versions/{versionName}/`
* Method: GET
* **Please note that `/` at the end is required**

Path parameters:

| Param          | Required | Description                                                                   |
|----------------|----------|-------------------------------------------------------------------------------|
| appId          | yes      | Unique identifier of the Application.                                         |
| platform       | yes      | Platform for this Application version. Valid options are `android` and `ios`. |
| versionName    | yes      | Unique version name for this Application and platform.                        |

Example request:

```http
GET /api/v1/configuration/applications/myApp/platforms/ios/versions/1.1.0/ HTTP/1.1
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
   "mobile_app_id": "ExampleApp",
   "platform": "ios",
   "version_name": "1.1.0",
   "status": "LOGIN_ONLY",
   "tampering_protection_enabled": false,
   "payload_encryption_enabled": true,
   "push_messaging_configuration_id": "f66d9bfc-7182-4c97-ae81-ddcd3f76a951",
   "use_apns_development_environment_enabled": true,
   "send_badge_number_enabled": true,
   "integrity_check": "FULL"
}
```

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### List of Application versions per platform

This returns a list of all Application versions for a given platform. 

* Endpoint: `/api/v1/configuration/applications/{appId}/platforms/{platform}/versions`
* Method: GET

Path parameters:

| Param          | Required | Description                                                                   |
|----------------|----------|-------------------------------------------------------------------------------|
| appId          | yes      | Unique identifier of the Application.                                         |
| platform       | yes      | Platform for this Application version. Valid options are `android` and `ios`. |

Example request:

```http
GET /api/v1/configuration/applications/myApp/platforms/ios/versions HTTP/1.1
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
           "mobile_app_id": "ExampleApp",
           "platform": "ios",
           "version_name": "1.1.0",
           "status": "LOGIN_ONLY",
           "tampering_protection_enabled": false,
           "payload_encryption_enabled": true,
           "push_messaging_configuration_id": "f66d9bfc-7182-4c97-ae81-ddcd3f76a951",
           "use_apns_development_environment_enabled": true,
           "send_badge_number_enabled": true,
           "integrity_check": "FULL"
       },
       {
           "mobile_app_id": "ExampleApp",
           "platform": "ios",
           "version_name": "1.10.0",
           "status": "LOGIN_REGISTRATION",
           "tampering_protection_enabled": false,
           "payload_encryption_enabled": true,
           "push_messaging_configuration_id": "c946a230-1567-4ff4-b182-2e2777ab0efa",
           "use_apns_development_environment_enabled": true,
           "send_badge_number_enabled": true,
           "application_bundle_identifier": "com.onegini.exampleapp",
           "integrity_check": "NONE"
       },
       {
           "mobile_app_id": "ExampleApp",
           "platform": "ios",
           "version_name": "1.11.0-nopush",
           "status": "LOGIN_REGISTRATION",
           "tampering_protection_enabled": false,
           "payload_encryption_enabled": true,
           "integrity_check": "FULL"
       }
  ]
}
```

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Create an Application version

This creates an Application version from scratch. 

* Endpoint: `/api/v1/configuration/applications/{appId}/platforms/{platform}/versions`
* Method: POST

Path parameters:

| Param          | Required | Description                                                                   |
|----------------|----------|-------------------------------------------------------------------------------|
| appId          | yes      | Unique identifier of the Application.                                         |
| platform       | yes      | Platform for this Application version. Valid options are `android` and `ios`. |

JSON body parameters:

| Param                                    | Required | Example                              | Description                                                                                                                                                                                                                                                       |
|------------------------------------------|----------|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| version_name                             | yes      | 1.0.0                                | Unique version name for this Application and platform.                                                                                                                                                                                                            |
| status                                   | yes      | LOGIN_ONLY                           | Defines usage of this Application version. Values are DISABLED, LOGIN_ONLY and LOGIN_REGISTRATION.                                                                                                                                                                |
| application_signature                    | yes      | "abdc"                               | (DEPRECATED, use `application_signatures`) Signature that is used to authenticate the Application version. Has [restrictions](../../../topics/mobile-apps/app-configuration/app-version-management.md) when tampering protection is enabled. Not returned on GET. |
| application_signatures                   | yes      | ["abdc", "defg"]                     | Signature(s) that are used to authenticate the Application version. Has [restrictions](../../../topics/mobile-apps/app-configuration/app-version-management.md) when tampering protection is enabled. Not returned on GET.                                        |
| integrity_check                          | no       | NONE                                 | Indicates if a full application integrity check is performed on mobile device. Available values are `FULL` and `NONE`. Default and recommended value is `FULL`.                                                                                                   |
| tampering_protection_enabled             | no       | true                                 | Flag to enable tampering protection. Defaults to `false`.                                                                                                                                                                                                         |
| payload_encryption_enabled               | no       | true                                 | Flag to enable payload encryption. Defaults to `false`.                                                                                                                                                                                                           |
| push_messaging_configuration_id          | no       | d10fe35f-ebb5-42bb-a81f-62a7034a68fb | Unique identifier of the push messaging configuration for this Application version.                                                                                                                                                                               |
| framework                                | no       | CORDOVA                              | For Android only. Set to `CORDOVA` when you send push messages to an Android app that is using Cordova. The actual push message will be sent via FCM for Cordova.                                                                                                 |
| use_apns_development_environment_enabled | no       | false                                | For iOS only. When set to `true` the push messages will be sent to the APNS development environment.                                                                                                                                                              |
| send_badge_number_enabled                | no       | true                                 | For iOS only. When set to `true` the number of unhandled push authentications are sent with the push request.                                                                                                                                                     |
| application_bundle_identifier            | depends  | com.example.myApp                    | For iOS only. Unique identifier for an Application. Its value can be found in the Apple developer console. Required for iOS push messaging configurations.                                                                                                        |

Example request:

```http
POST /api/v1/configuration/applications/myApp/platforms/ios/versions HTTP/1.1
Host: onegini.example.com
Content-Type: application/json

{
  "version_name": "1.0.0",
  "application_signatures": ["abdc", "defg"],
  "tampering_protection_enabled": false,
  "payload_encryption_enabled": true,
  "status": "LOGIN_REGISTRATION",
  "push_messaging_configuration_id": "6727cf80-2807-11e6-b47f-89550f4d53bb",
  "use_apns_development_environment_enabled": false,
  "send_badge_number_enabled": true,
  "application_bundle_identifier": "com.example.myApp",
  "integrity_check": "FULL",
}
```

Example success response:

```http
HTTP/1.1 201 CREATED
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
Location: /api/v1/configuration/applications/myApp/platforms/ios/versions/1.1.0/
```

The success response body is empty.

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Clone an Application version

This creates a new Application version with values from an existing Application version. This can be useful when the new Application version needs the same 
settings for payload encryption, tampering protection or push configuration.

* Endpoint: `/api/v1/configuration/applications/{appId}/platforms/{platform}/versions/{originalVersionName}/clone`
* Method: POST

Path parameters:

| Param               | Required | Description                                                                   |
|---------------------|----------|-------------------------------------------------------------------------------|
| appId               | yes      | Unique identifier of the Application.                                         |
| platform            | yes      | Platform for this Application version. Valid options are `android` and `ios`. |
| originalVersionName | yes      | Unique version name for this Application and platform that is being cloned.   |

JSON body parameters:

| Param                            | Required | Example           | Description                                                                                                                                                                                                                                                                                   |
|----------------------------------|----------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| version_name                     | yes      | 1.0.1             | Unique version name for this Application and platform.                                                                                                                                                                                                                                        |
| application_signature            | no       | "abdc"            | DEPRECATED (use `application_signatures`)                                                                                                                                                                                                                                                     |
| application_signatures           | no       | ["abdc", "defg"]  | Signature(s) that are used to authenticate the Application version. Has [restrictions](../../../topics/mobile-apps/app-configuration/app-version-management.md) when tampering protection is enabled. Not returned on GET. If not specified, all signatures are copied from original version. |
| status                           | no       | LOGIN_ONLY        | Defines usage of this Application version. Values are DISABLED, LOGIN_ONLY and LOGIN_REGISTRATION.                                                                                                                                                                                            |

Example request:

```http
POST /api/v1/configuration/applications/myApp/platforms/ios/versions/3.0.0/clone HTTP/1.1
Host: onegini.example.com
Content-Type: application/json

{
  "version_name": "3.0.1",
  "application_signatures": ["abdc"],
  "status": "DISABLED"
}
```

Example success response:

```http
HTTP/1.1 201 CREATED
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
Location: /api/v1/configuration/applications/myApp/platforms/ios/versions/3.0.1/
```

The success response body is empty.

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Update an Application version

Some fields can be updated after creating an Application version. 

* Endpoint: `/api/v1/configuration/applications/{appId}/platforms/{platform}/versions/{versionName}/`
* Request method: PATCH
* **Please note that `/` at the end is required**

Path parameters:

| Param          | Required | Description                                                                   |
|----------------|----------|-------------------------------------------------------------------------------|
| appId          | yes      | Unique identifier of the Application.                                         |
| platform       | yes      | Platform for this Application version. Valid options are `android` and `ios`. |
| versionName    | yes      | Unique version name for this Application and platform.                        |

JSON body parameters:

Only the fields that are sent in the request will be changed.

| Param                                    | Required | Example                              | Description                                                                                                                                                                                                                                                       |
|------------------------------------------|----------|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status                                   | no       | LOGIN_ONLY                           | Defines usage of this Application version. Values are DISABLED, LOGIN_ONLY and LOGIN_REGISTRATION.                                                                                                                                                                |
| application_signatures                   | no       | abdc"                                | (DEPRECATED, use `application_signatures`) Signature that is used to authenticate the Application version. Has [restrictions](../../../topics/mobile-apps/app-configuration/app-version-management.md) when tampering protection is enabled. Not returned on GET. |
| application_signatures                   | no       | ["abdc", "defg"]                     | Signature(s) that ARE used to authenticate the Application version. Has [restrictions](../../../topics/mobile-apps/app-configuration/app-version-management.md) when tampering protection is enabled. If specified, it cannot be empty. Not returned on GET.      |
| integrity_check                          | no       | NONE                                 | Indicates if a full application integrity check is performed on mobile device. Available values are `FULL` and `NONE`.                                                                                                                                            |
| payload_encryption_enabled               | no       | true                                 | Flag to enable payload encryption. It can be updated only if the SDK supports JWT authentication.                                                                                                                                                                 |
| push_messaging_configuration_id          | no       | d10fe35f-ebb5-42bb-a81f-62a7034a68fb | Unique identifier of the push messaging configuration for this Application version. Use empty value to clear the field.                                                                                                                                           |
| framework                                | no       | CORDOVA                              | For Android only. Set to `CORDOVA` when you send push messages to an Android app that is using Cordova. The actual push message will be sent via FCM for Cordova.                                                                                                 |
| use_apns_development_environment_enabled | no       | false                                | For iOS only and requires a push messaging configuration to be set. When set to `true` the push messages will be sent to the APNS development environment.                                                                                                        |
| send_badge_number_enabled                | no       | true                                 | For iOS only and requires a push messaging configuration to be set. When set to `true` the number of unhandled push authentications are sent with the push request.                                                                                               |
| application_bundle_identifier            | depends  | com.example.myApp                    | For iOS only and requires a push messaging configuration to be set. Unique identifier for an Application. Its value can be found in the Apple developer console. Required for iOS push messaging configurations. Use empty value to clear the field.              |

Example request:

```http
PATCH /api/v1/configuration/applications/myApp/platforms/ios/versions/3.0.0/ HTTP/1.1
Host: onegini.example.com
Content-Type: application/json

{
  "application_signatures": ["123456789012345678901234567890AB"],
  "status": "LOGIN_REGISTRATION"
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

### Export an Application version config

This endpoint produces a ZIP file with the configuration that is needed by the Onegini SDK.

* Endpoint: `/api/v1/configuration/applications/{appId}/platforms/{platform}/versions/{versionName}/export`
* Method: GET

Path parameters:

| Param          | Required | Description                                                                   |
|----------------|----------|-------------------------------------------------------------------------------|
| appId          | yes      | Unique identifier of the Application.                                         |
| platform       | yes      | Platform for this Application version. Valid options are `android` and `ios`. |
| versionName    | yes      | Unique version name for this Application and platform.                        |


Example Request:
```http
GET /api/v1/configuration/applications/myApp/platforms/ios/versions/3.0.0/export HTTP/1.1
Host: onegini.example.com
application/zip;charset=UTF-8
```

Success response:
```http
HTTP/1.1 200 
Content-Type: application/zip;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
```

The response body contains a ZIP file which is removed from the example for readability.

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

## Error codes

One of the following responses will be returned, containing a JSON object with an error code, a message and details about the error.

| HTTP status | Error code                       | Message                                                                                                  |
|-------------|----------------------------------|----------------------------------------------------------------------------------------------------------|
| 400         | invalid_request                  | One or more parameters is missing or incorrect. The details contain the missing or incorrect parameters. |
| 401         | unauthorized                     | Provide valid credentials to get access to the API.                                                      |
| 403         | forbidden                        | Operation is not allowed for the current user.                                                           |
| 404         | not_found                        | Mobile App, Platform, or Version cannot be found                                                         |
| 409         | conflict                         | An application version with the same appId, platform, and version already exists.                        |
