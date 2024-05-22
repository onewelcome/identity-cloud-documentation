# Service Providers API

This allows to configure a Service Provider via a REST API.
It can be utilized in scripts to add, list, edit, or delete service provider configurations.

All endpoints are protected with API client.
It requires an API client with the `onegini_api_config` scope (Config API).

## Service Provider configuration JSON body parameters

JSON body parameters used in the [Create](#Create-Service-Provider-configuration)
and [Update](#Update-Service-Provider-configuration) requests:

| Param             | Required | Example                           | Description                                                                                                                       |
|-------------------|----------|-----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| id                | no       | "431eb5b7-c8e2-4b67-b735-ed95a1"  | Unique identifier for the SP configuration. If not specified, it will be generated.                                               |
| name              | yes      | "Service Provider name"           | Service Provider name.                                                                                                            |
| metadata_type     | yes      | "XML"                             | Type of Service Provider metadata. <br/> Allowed values: `URL`, `XML`.                                                            |
| metadata_xml      | depends  | XML file content                  | Required when metadata type is XML.                                                                                               |
| metadata_url      | depends  | "https://example.sp.com/metadata" | Location of the remote XML metadata. Required when metadata type is URL.                                                          |
| identity_provider | yes      | "onewelcome_idp"                  | Id of the default Identity Provider.                                                                                              |
| oauth_scopes      | no       | ["openid", "profile"]             | Scopes that will be sent with authentication requests to the selected Identity Provider. It applies only to the OAuth based IDPs. |

## Endpoints

### List of configured Service Providers

This returns a list of all configured Service Providers.

* Endpoint: `/api/v1/configuration/saml-sp`
* Method: GET

Example request:

```http
GET /api/v1/configuration/saml-sp HTTP/1.1
Host: onewelcome.example.com
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
            "id": "e465d256-fd78-44db-8645-c23a78349251",
            "name": "Service Provider name",
            "entity_id": "https://service.provider.com",
            "metadata_type": "XML"
        },
        {
        ... more service provider configurations ...
        }
   ]
}
```

In the event of an error, one of the [generic error codes](#error-codes) will be returned.

### Create Service Provider configuration

This creates a new Service Provider configuration

* Endpoint: `/api/v1/configuration/saml-sp`
* Method: POST

#### Limitations

1. Entity ID must be unique in the scope of one tenant.
2. The `identity_provider` field can only refer to existing identifiers of Identity Providers.
   When an unknown identifier is passed, the response will contain a validation error: `Identity provider [provided_id] not found`.
3. In the case of `URL` and `XML` metadata type, when `validUntil` attribute is specified in Entity Descriptor, Access will check the
   metadata
   validity and return an error in case of expired metadata.
4. In the case of `URL` and `XML` metadata type, when the XML metadata is signed, Access will check the signature validity and return an
   error in case of failed verification.
5. Provided metadata must contain `SPSSODescriptor` element containing at least one `AssertionConsumerService`.
6. The max length of `name` and `entity_id` fields is 255.

Example request:

```http
POST /api/v1/configuration/saml-sp HTTP/1.1
Host: onewelcome.example.com
Content-Type: application/json
{
    "name": "My Service Provider",
    "metadata_type": "URL",
    "metadata_url": "https://example.sp.com/metadata.xml",
    "identity_provider": "oidc_idp",
    "oauth_scopes": [ "openid", "profile", "email" ]
}
```

Example success response:

```http
HTTP/1.1 201 CREATED
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
Location: /api/v1/configuration/saml-sp/60eb60e3-01e0-496e-87f4-fd29737a8881
{
    "id": "60eb60e3-01e0-496e-87f4-fd29737a8881",
    "name": "My Service Provider",
    "entity_id": "myEntityId",
    "metadata_type": "URL"
}
```

Create Service Provider configuration request is validated. In case of failure, a list of validation errors is returned.
Example failed response:

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
{
    "validation_errors": [
        {
            "filed": "identity_provider",
            "message": "Identity provider [oidc_idp] not found."
        },
        {
            "filed": "assertion_consumer_service_location",
            "message": "Field must be a valid URL."
        }
    ]
}
```

### Read Service Provider configuration

This returns detailed Service Provider configuration object.

* Endpoint: `/api/v1/configuration/saml-sp/{service_provider_id}`
* Method: GET

Path parameters:

| Param               | Required | Description                                              |
|---------------------|----------|----------------------------------------------------------|
| service_provider_id | yes      | Unique identifier of the Service Provider configuration. |

Example request:

```http
GET /api/v1/configuration/saml-sp/120b0682-8ddd-41fc-9ec9-6f76c48afc08 HTTP/1.1
Host: onewelcome.example.com
```

Example success response:

```http
HTTP/1.1 200 Ok
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
{
    "id": "120b0682-8ddd-41fc-9ec9-6f76c48afc08",
    "name": "My Service Provider",
    "entity_id": "myEntityId",
    "metadata_type": "URL",
    "metadata_url": "https://example.sp.com/metadata.xml",
    "identity_provider": "custom",
    "oauth_scopes": [],
    "parsed_metadata": {
        "assertion_consumer_services": [
            {
                "type": "POST",
                "location": "https://example.sp.com/authn-response"
            }
        ],
        "default_assertion_consumer_service": {
            "type": "POST",
            "location": "https://example.sp.com/authn-response"
        },
        "single_logout_services": [
            {
                "type": "POST",
                "location": "https://example.sp.com/logout"
            }
        ],
        "signing_certificate": [
            {
                "issuer": "CN=OneWelcome",
                "expiration": "2031-06-10T06:38:30Z",
                "fingerprint": "26:DC:42:84:CE:45:59:04:44:60:23:35:05:E4:F8:3F:25:9C:94:03"
            }
        ],
        "encryption_certificate": []
    }
}
```

### Update Service Provider configuration

Service Provider configuration fields can be updated after creating a Service Provider configuration.

* Endpoint: `/api/v1/configuration/saml-sp/{service_provider_id}`
* Method: PATCH

Path parameters:

| Param               | Required | Description                                              |
|---------------------|----------|----------------------------------------------------------|
| service_provider_id | yes      | Unique identifier of the Service Provider configuration. |

Only the fields that are sent in the request will be changed.

Example request:

```http
PATCH /api/v1/configuration/saml-sp/60eb60e3-01e0-496e-87f4-fd29737a8881 HTTP/1.1
Host: onewelcome.example.com
Content-Type: application/json
{
    "name": "Updated name",
    "metadata_type": "URL",
    "metadata_url": "https://example.sp.com/saml/metadata"
}
```

Example success response:

```http
HTTP/1.1 200 
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
{
    "id": "60eb60e3-01e0-496e-87f4-fd29737a8881",
    "name": "Updated name",
    "entity_id": "https://example.sp.com",
    "metadata_type": "URL"
}
```

Update Service Provider configuration request is validated. In case of validation failure, a list of validation errors is returned.
Example failed response:

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
{
    "validation_errors": [
        {
            "filed": "metadata_url",
            "message": "The entity ID (https://example.sp.com) is already used by the 'my_service_provider' Service Provider."
        }
    ]
}
```

### Delete Service Provider configuration

This removes a Service Provider configuration.

* Endpoint: `/api/v1/configuration/saml-sp/{service_provider_id}`
* Method: DELETE

Path parameters:

| Param               | Required | Description                                              |
|---------------------|----------|----------------------------------------------------------|
| service_provider_id | yes      | Unique identifier of the Service Provider configuration. |

Example request:

```http
DELETE /api/v1/configuration/saml-sp/60eb60e3-01e0-496e-87f4-fd29737a8881 HTTP/1.1
Host: onewelcome.example.com
```

Example success response:

```http
HTTP/1.1 204 NO CONTENT
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
```

## Error codes

One of the following responses will be returned, containing a JSON object with an error code, a message, and details about the error.

| HTTP status | Error code   | Message                                                                     |
|-------------|--------------|-----------------------------------------------------------------------------|
| 401         | unauthorized | Provide valid credentials to get access to the API.                         |
| 403         | forbidden    | Operation is not allowed for the current user.                              |
| 404         | not_found    | Service Provider configuration cannot be found for this service_provider_id |
