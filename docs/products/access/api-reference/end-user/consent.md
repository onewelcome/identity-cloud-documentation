# Consent API

The consent API provides consent management capabilities to the web application, possibly on behalf of the end user. For example, the web application may use these endpoints to list the consents to the end user, and provide the user with the option to delete specific consents.

## List consents

Endpoint: `GET /oauth/api/v1/users/{userId}/consents`

| Parameter | Description            |
|-----------|------------------------|
| `userId`  | User identifier.       |

This endpoint requires basic authentication using valid API client credentials.

When no consents are found because the user does not exist or the user does not have consents, a `404 Not Found` is returned. When the user does have consents, an array with one or more consents is returned. Each consent object has the following attributes:
 
| Attribute       | Description                                                                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `id`            | UUID identifying the consent.                                                                                                                               |
| `client_name`   | Name specified for the client that was given consent to request an access grant for given scope(s).                                                         |
| `device_name`   | Name of the device to which this token is granted. (May be not present as this attribute only contains a value in case of a dynamically registered client.) |
| `scopes`        | String array with scopes for which the user has given consent for.                                                                                          |

**Example result:**

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
 
{
  "consents": [
    {
      "id": "fc4ef972-7167-4421-aa89-f109be79d7c2",
      "client_name": "Client X",
      "device_name": "My iPad",
      "scopes": [
        "email",
        "profile"
      ]
    },
    {
      "id": "451f5c34-3d03-4ce0-80bd-4676fc0eddf5",
      "client_name": "Client Y",
      "scopes": [
        "email"
      ]
    }
  ]
}
```

**Example error response:**

```json
{
  "error": "No consents found"
}
```

## Delete / revoke consent

Endpoint: `DELETE /oauth/api/v1/users/{userId}/consents/{consentId}`

| Parameter   | Description               |
|-------------|---------------------------|
| `userId`    | Identifier of the user    |
| `consentId` | Identifier of the consent |

This endpoint requires basic authentication using valid API client credentials.

Deleting a consent for a specific user & client will also remove the corresponding access grant and access token when available. This endpoint always returns a `204 No Content` response, independent of the execution result.

**Example result:**

```http
HTTP/1.1 204 No Content
Cache-Control: no-store
Pragma: no-cache
```
