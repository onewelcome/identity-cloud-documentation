# Device API version 3 (deprecated)

**This version of the Device API is deprecated. It is replaced by [Device API v4](./device-v4.md).**

The device API enables a web application to list devices that a user has (dynamically) registered, possibly on behalf of the user. This API can also be used to delete (deregister) a device entirely. For example, a user that loses a device may wish to revoke all permissions and access belonging to that device.

All endpoints are protected with API client credentials, either Client Secret Basic or PrivateKeyJWT depending on the client [authentication method](../../topics/authentication-methods/authentication-methods.md). It requires an API client with the scope `end_user_api` (End User API).

## List devices

The list devices endpoint can be used to display information on each of the devices a user has registered.

Endpoint: `GET /oauth/api/v3/users/{userId}/devices`

| Parameter | Description            |
|-----------|------------------------|
| `userId`  | Identifier of the user |

When no devices are found because the user does not exist or the user does not have any devices, a `404 Not Found` is returned. When the user does have devices, an array with one or more devices is returned. Each device object has the following attributes.

| Attribute                       | Description                                                                                  
|---------------------------------|----------------------------------------------------------------------------------------------
| `id`                            | Identifier of the device.                                                                    
| `name`                          | The device name                                                                              
| `application`                   | A web client / group of dynamic clients that use the same configuration                      
| `platform`                      | Platform / os of the device. Possible values: `android` and `ios`.                            
| `created_at`                    | Timestamp when the device was registered; the value can be empty.
| `token_types`                   | List of access token types issued for a device. Possible values `DEFAULT`, `FINGER_PRINT`, `CUSTOM_AUTHENTICATOR` and `IMPLICIT_AUTHENTICATION`.
| `last_login`                    | Date of the last login using a device                                                        
| `mobile_authentication_enabled` | `true` if a device is enrolled for [mobile authentication](../../topics/mobile-apps/mobile-authentication/mobile-authentication.md)                                     
| `push_authentication_enabled`   | `true` if a device is enrolled for mobile authentication with push                           

**Example result:**

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
 
{
  "devices": [
    {
      "id": "05ED8E51CB1EFAA2DBCECC90504ADC1C11BEB7CE3C6D68065FBF7E7E86980CF3",
      "name": "Jane's Android Phone",
      "application": "application 1",
      "platform": "android",
      "created_at": 1381322054000,
      "last_login": 1548929031000,
      "token_types": [
        "DEFAULT"
      ],
      "mobile_authentication_enabled": true,
      "push_authentication_enabled": true        
    },
    {
      "id": "7A8A520DB50864F1DA3F12FC6692D1267535339E76CE041405D4CED2449DA858",
      "name": "Mallory's iPhone 📱",
      "application": "application 2",
      "created_at": 1381322054000,
      "platform": "ios",
      "last_login": 1556276182000,
      "token_types": [
        "DEFAULT"
      ],
      "mobile_authentication_enabled": true,
      "push_authentication_enabled": false   
    }
  ]
}
```

**Example error response:**

```http
HTTP/1.1 404 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
{
  "error": "No devices found"
}
```

## Delete or revoke device

The delete device endpoint can be used to delete a device from OneWelcome Access. This completely deregisters the user from the device meaning that all 
access grants, access  tokens, registered custom authentications, and mobile authentication enrollments are revoked on OneWelcome Access. Note that these
assets will remain on the device itself, but they will not be valid anymore on the server side.

Endpoint: `DELETE /oauth/api/v3/users/{userId}/devices/{deviceId}`

| Parameter  | Description              |
|------------|--------------------------|
| `userId`   | Identifier of the user   |
| `deviceId` | Identifier of the device |

This endpoint always returns a `204 No Content` response, independent of the execution result. When there are multiple users (profiles) registered on a device, 
only data related to the provided `userId` will be deleted. 

**Example result:**

```http
HTTP/1.1 204 No Content
Cache-Control: no-store
Pragma: no-cache
```

## Bulk delete or revoke all devices of a user

The bulk delete device endpoint deletes all devices enrolled for a specific user. See [Delete or revoke device](#delete-or-revoke-device) for further details.

Endpoint: `DELETE /oauth/api/v3/users/{userId}/devices`

| Parameter  | Description              |
|------------|--------------------------|
| `userId`   | Identifier of the user   |

This endpoint returns a `204 No Content` response when all devices for this user have been revoked successfully.  

**Example result:**

```http
HTTP/1.1 204 No Content
Cache-Control: no-store
Pragma: no-cache
```

## Bulk delete or revoke a selection of devices of a user

The bulk delete device endpoint deletes a selection of devices enrolled for a specific user. See [Delete or revoke device](#delete-or-revoke-device) for further 
details. This method reduces the number of API calls when deleting multiple devices at the same time. 

Endpoint: `POST /oauth/api/v3/users/{userId}/devices`

Path parameters:

| Parameter  | Description              |
|------------|--------------------------|
| `userId`   | Identifier of the user   |

JSON body parameters:

| Parameter  | Required | Description                                |
|------------|----------|--------------------------------------------|
| delete     | yes      | Array of identifiers for devices to delete |


Example request:

```http
POST /oauth/api/v3/users/myDummyUserId/devices
Host: onewelcome.example.com
Accept: application/json
Content-Type: application/json

{
  "delete": [
    "05ED8E51CB1EFAA2DBCECC90504ADC1C11BEB7CE3C6D68065FBF7E7E86980CF3",
    "7A8A520DB50864F1DA3F12FC6692D1267535339E76CE041405D4CED2449DA858"
  ]
}
```

This endpoint returns a `204 No Content` response when all devices from the request have been revoked successfully.  

**Example result:**

```http
HTTP/1.1 204 No Content
Cache-Control: no-store
Pragma: no-cache
```

**Example error response:**

```http
HTTP/1.1 500 Internal Server Error
Cache-Control: no-cache, no-store, must-revalidate
Pragma: no-cache
{
    "code": "not_all_devices_deleted",
    "message": "Some of the devices could not be deleted.",
    "details": [
        {
            "id": "unknown_device_id",
            "status": {
                "code": "device_not_deleted",
                "message": "The device could not be deleted.",
                "details": []
            }
        }
    ]
}
```

## Disable fingerprint

The disable fingerprint endpoint can be used to revoke a fingerprint refresh token on a user's device. Note that this endpoint does not prevent a user from registering their fingerprint again.

Endpoint: `POST /oauth/api/v3/users/{userId}/devices/{deviceId}/disableFingerprint`

| Parameter  | Description              |
|------------|--------------------------|
| `userId`   | Identifier of the user   |
| `deviceId` | Identifier of the device |

This endpoint always returns a `204 No Content` response, independent of the execution result.

## Disable mobile authentication

The disable mobile authentication can be used to withdraw a user's device for mobile authentication. When the SDK enrolls a user for mobile authentication, the SDK and the OneWelcome Access exchange public keys. The SDK must take an additional step to enroll for mobile authentication with push (and provide a push token).

When this endpoint is called for a user's device, the OneWelcome Access withdraws the user's device for all types of mobile authentication (including push). The public keys are removed server side, and the push token (if it exists) is also dropped.

Note that this action does not prevent a user from enrolling the same device a second time for mobile authentication.

Endpoint: `POST /oauth/api/v3/users/{userId}/devices/{deviceId}/disableMobileAuthentication`

| Parameter  | Description              |
|------------|--------------------------|
| `userId`   | Identifier of the user   |
| `deviceId` | Identifier of the device |

This endpoint always returns a `204 No Content` response, independent of the execution result.

## Disable mobile authentication with push

The disable mobile authentication with push endpoint can be used to disenroll a user's device for push only. It removes the push token from the OneWelcome Access, but leaves the user's public key intact, allowing other forms of mobile authentication to remain working.

Note that this action does not prevent a user from re-enrolling for mobile authentication with push.

Endpoint: `POST /oauth/api/v3/users/{userId}/devices/{deviceId}/disablePushAuthentication`

| Parameter  | Description              |
|------------|--------------------------|
| `userId`   | Identifier of the user   |
| `deviceId` | Identifier of the device |

This endpoint always returns a `204 No Content` response, independent of the execution result.

One of the following responses will be returned, containing a JSON object with an error code and a message with details about the error.

| HTTP status | Error code                       | Details 
|-------------|----------------------------------|-------------------------------------------------------------------------------------
| 400         | invalid_request                  | One or more parameters is missing or incorrect. The details contain the missing or incorrect parameters.
| 401         | unauthorized                     | Provide valid credentials to get access to the API.
| 403         | insufficient_permissions         | The API client used to authenticate the request does not allow operations on the End User API. 
| 404         | not_found                        | The device does not exist or does not belong to the user.
| 500         | not_all_devices_deleted          | The deletion of one or more devices failed during a bulk operation. The other devices have been removed.
