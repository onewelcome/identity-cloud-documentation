# User Session API

The User Session API provides capabilities for managing user sessions. Like the endpoints in the end user API, the session endpoints are
only accessible with valid API client credentials. These endpoints can be utilized by a web application to list active sessions for a
specific user. For instance, the web application might want to display a list of active sessions to the user, including details such
as session ID, authentication time, last issued access time, user agent, and location information.

In addition to listing sessions, this API also allows for terminating user sessions.
These termination endpoints provide a layer of security and control over user sessions. It ensures old and inactive sessions can be properly
ended as needed, maintaining the integrity of the user's active sessions.

## List User Sessions

Endpoint: `GET /oauth/api/v1/users/{userId}/sessions`

| Parameter | Description     |
|-----------|-----------------|
| `userId`  | User identifier |

This endpoint requires basic authentication using the API client credentials.
If the user does not exist, or if the user has no active sessions, a `404 Not Found` is returned.
If there are active sessions, a response is returned with an array of session details.

| Attribute                | Description                                                                                                                         |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| `session_id`             | Identifier of the session.                                                                                                          |
| `auth_time`              | A timestamp indicating when the user was authenticated in this session.                                                             |
| `last_iat`               | A timestamp indicating when the last Access Token was issued for the user in this session.                                          |
| `user_agent`             | A string indicating the user agent of the device used in the session.                                                               |
| `location`               | An object containing location information such as IP address.                                                                       |
| `clients`                | An array of objects representing the clients associated with the session, including client ID and name.                             |
| `saml_service_providers` | An array of objects representing the saml service providers associated with the session, including provider id, entity id and name. |

**Example response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
 
{
  "result": [
    {
      "session_id": "8f4ecb2b-7bc1-47bc-95e1-0b02ae4b6e32",
      "auth_time": "2023-11-13T09:31:49.231460Z",
      "last_iat": "2023-11-13T09:31:49.340Z",
      "user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/119.0",
      "location": {
        "ip_address": "10.0.0.1"
      },
      "clients": [
        {
          "id": "my-client",
          "name": "my-client"
        }
      ],
      "saml_service_providers": [
        {
           "id": "4fa0da36-be99-469a-a40e-155f13e70e3e",
           "name": "Test SP",
           "entity_id": "http://default.sp"
        }
      ]
    }
  ]
}
```

**Example error response:**

```json
{
  "error": "No sessions found"
}
```

## End User Sessions

### End All Sessions

Endpoint: `DELETE /oauth/api/v1/users/{userId}/sessions`

| Parameter | Description              |
|-----------|--------------------------|
| `userId`  | User's unique identifier |

This secure endpoint requires authentication using API credentials. It has been designed to delete every active session of a specified user.
Default behavior is to remove the associated tokens as well.

Upon successful deletion, a `204 No Content` status is sent back. If the user doesn't exist or has no active sessions, it will also result
in `204 No Content` being returned.

**Query Parameters:**

| Parameter      | Description                                                | Default |
|----------------|------------------------------------------------------------|---------|
| `removeTokens` | If true, additionally scraps all of the associated tokens. | `true`  |

---

### End a Specific Session

Endpoint: `DELETE /oauth/api/v1/users/{userId}/sessions/{sessionId}`

| Parameter   | Description                 |
|-------------|-----------------------------|
| `userId`    | User's unique identifier    |
| `sessionId` | Session's unique identifier |

This endpoint, protected by authentication via client credentials, removes an individual session of the user in question. By default, the
associated tokens are removed too.

Upon successful deletion, you'll receive a `204 No Content` status. If the user or the session is nonexistent, a status `204 No Content` is
sent back as well.

**Query Parameters:**

| Parameter      | Description                                                        | Default |
|----------------|--------------------------------------------------------------------|---------|
| `removeTokens` | If true, cleans out all of the tokens associated with the session. | `true`  |

Both of these endpoints are invaluable security tools that help you administer user sessions and ensure that older, unused sessions are
appropriately terminated thereby enhancing your control over the process of user authentication.
