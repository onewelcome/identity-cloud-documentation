# Application API

The application API provides application management capabilities to the portal (web application). An application can be a web client or a group of dynamic clients that use the same configuration (e.g. the instances that are installed on a tablet and on a mobile phone are separate dynamic clients but refer to the same application).

## List applications

Endpoint: `GET /oauth/api/v1/users/{userId}/applications`

| Parameter | Description            |
|-----------|------------------------|
| `userId`  | identifier of the user |

This endpoint requires basic authentication with valid API client credentials.

When no applications are found because the user does not exist or the user does not have any applications, a `404 Not Found` is returned. When the user does have applications, an array with one or more applications is returned. Each applications object has the following attributes:

| Attribute | Description                      |
|-----------|----------------------------------|
| `id`      | UUID identifying the application |
| `name`    | The application name             |

**Example result:**

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
 
{
  "apps": [
    {
      "id": "69cd25e7-ec86-43e2-8714-8fbb1d7583ec",
      "app_name": "Application X"
    },
    {
      "id": "408a9d9a-2b25-4c7a-8e2c-038150ab040c",
      "app_name": "Application Y"
    }
  ]
}
```

**Example error response:**

```json
{
  "error": "No applications found"
}
```

## Delete / revoke access to application

By deleting access to an application for a user, all access tokens, access grants & consents a user has for any instance of the application will be removed.

Endpoint: `DELETE /oauth/api/v1/users/{userId}/applications/{applicationId}`

| Parameter       | Description                      |
|-----------------|----------------------------------|
| `userId`        | Identifier of the user           |
| `applicationId` | UUID identifying the application |

This endpoint requires basic authentication with valid API client credentials.

The API always returns a `204 No Content` response, independent of the execution result.
