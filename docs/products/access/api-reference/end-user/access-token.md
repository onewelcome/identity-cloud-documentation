# Access token API

The access token API provides access token management capabilities. Like all endpoints in the end user API, the token endpoints are only
accessible with valid API client credentials. These endpoints can be used by a web application to list active tokens for a given user, or
delete them. For example, the web application may wish to show the user a list of devices with authenticated sessions, and allow the user to
revoke access for a device (by deleting the relevant token).

## List access tokens

Endpoint: `GET /oauth/api/v1/users/{userId}/tokens`

| Parameter | Description     |
|-----------|-----------------|
| `userId`  | User identifier |

This endpoint requires basic authentication, using the API client credentials. If the user does not exist, or if the user has no valid
access tokens, a `404 Not Found` is returned. If the user has one or more valid tokens, an array is returned with the following attributes.

| Attribute              | Description                                                                                                                                                 |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `id`                   | UUID identifying the token.                                                                                                                                 |
| `client_name`          | Name specified for the client that has access to the user's resources via this token.                                                                       |
| `device_name`          | Name of the device to which this token is granted (may be not present, as this attribute only contains a value in case of a dynamically registered client). |
| `created_at`           | Timestamp of the moment the access token was created.                                                                                                       |
| `scopes`               | String array with scopes that were granted for this access token.                                                                                           |
| `type`                 | The authentication method to be used with this access token.                                                                                                |
| `refresh_token_issued` | Indicates whether a Refresh Token has been issued alongside the given Access Token.                                                                         |
| `expired`              | Indicates if the Access Token has expired. Tokens without an issued Refresh Token are omitted from the response once expired                                |

**Example response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
 
{
  "tokens": [
    {
      "id": "7d507b7e-6221-4f06-a75e-ef6e6f06d32b",
      "client_name": "Client X",
      "device_name": "my iPad",
      "created_at": 1381322054000,
      "scopes": [
        "email",
        "profile"
      ],
      "type":"DEFAULT",
      "refresh_token_issued": true,
      "expired": false
    },
    {
      "id": "1c05119e-21b2-4905-bc93-8f67790a16d6",
      "client_name": "Client Y",
      "created_at": 1381321302000,
      "scopes": [
        "email"
      ],
      "type":"FINGER_PRINT",
      "refresh_token_issued": true,
      "expired": false
    }
  ]
 }
```

**Example error response:**

```json
{
  "error": "No tokens found"
}
```

## Delete / revoke access token

Endpoint: `DELETE /oauth/api/v1/users/{userId}/tokens/{tokenId}`

| Parameter | Description                    |
|-----------|--------------------------------|
| `userId`  | Identifier of the user         |
| `tokenId` | Identifier of the access token |

This endpoint requires basic authentication, using the API client credentials. This endpoint returns `204 No Content` regardless of whether
the user and/or token existed before deletion.
