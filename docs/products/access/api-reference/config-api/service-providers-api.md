# Service Providers API

This allows to configure a Service Provider via a REST API.
It can be utilized in scripts to add, list, edit, or delete service provider configurations.

All endpoints are protected with API client.
It requires an API client with the `onegini_api_config` scope (Config API).

## Service Provider configuration JSON body parameters

JSON body parameters used in the [Create](#Create-Service-Provider-configuration)
and [Update](#Update-Service-Provider-configuration) requests:

| Param                     | Required | Example                           | Description                                                                      |
|---------------------------|----------|-----------------------------------|----------------------------------------------------------------------------------|
| name                      | yes      | "Service Provider name"           | Service Provider name.                                                           |
| metadata_type             | yes      | "XML"                             | Type of Service Provider metadata. <br/> Allowed values: `URL`, `XML`, `MANUAL`. |
| metadata_xml              | depends  | XML file content                  | Required when metadata type is XML.                                              |
| metadata_url              | depends  | "https://example.sp.com/metadata" | Location of the remote XML metadata. Required when metadata type is URL.         |
| manual_metadata           | depends  | See table below                   | Metadata in form of JSON object. Required when metadata type is MANUAL.          |
| user_identifier           | yes      | "Email address"                   | Format of the user identifier.                                                   |
| attribute_mappings        | no       | { "email" : "Email address" }     | Attribute mappings in form of key-value map.                                     |
| identity_provider         | yes      | "onewelcome_idp"                  | Id of the default Identity Provider.                                             |
| backup_identity_providers | no       | ["backup_idp_id"]                 | Ids of the backup Identity Providers.                                            |

The following parameters are a part of the `manual_metadata` object.

| Param                               | Required | Example                                          | Description                                                                                              |
|-------------------------------------|----------|--------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| entity_id                           | yes      | "https://example.sp.com"                         | Entity ID.                                                                                               |
| assertion_consumer_service_location | yes      | "https://example.sp.com/authn-response"          | Location of the Assertion Consumer Service.                                                              |
| assertion_consumer_service_binding  | yes      | "urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" | Binding type of the Assertion Consumer Service. Allowed bindings: `POST`, `Redirect`, `Artifact`, `PAOS` |
| signing_certificate                 | yes      | "-----BEGIN CERTIFICATE-----\r\nMIIEnTCCAoC ..." | Plain certificate for assertions signing in x509 format.                                                 |
| encryption_certificate              | no       | "-----BEGIN CERTIFICATE-----\r\nMIIEnTCCAoC ..." | Plain certificate for encryption in x509 format.                                                         |
| logout_url_redirect                 | no       | "https://example.sp.com/logout"                  | Location of the Single Logout Service (POST binding).                                                    |
| logout_url_post                     | no       | "https://example.sp.com/logout_post"             | Location of the Single Logout Service (Redirect binding).                                                |

## Endpoints

### List of configured Service Providers

This returns a list of all configured Service Providers.

* Endpoint: `/api/v1/saml-sp-config`
* Method: GET

Example request:

```http
GET /api/v1/saml-sp-config HTTP/1.1
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

* Endpoint: `/api/v1/saml-sp-config`
* Method: POST

#### Limitations

1. Entity ID must be unique in the scope of one tenant.
2. The `identity_provider` and `backup_identity_providers` fields are references to other parts in the configuration of Access.
   This API can only refer to existing identifiers of Identity Providers. When an unknown identifier is passed, the response will contain
   a validation error: `Identity provider [provided_id] not found`.
3. In the case of `URL` and `XML` metadata type, when `validUntil` attribute is specified in Entity Descriptor, Access will check the
   metadata
   validity and return an error in case of expired metadata.
4. In the case of `URL` and `XML` metadata type, when the XML metadata is signed, Access will check the signature validity and return an
   error in case of failed verification.
5. Provided metadata must contain `SPSSODescriptor` element, containing a signing certificate, and at least one `AssertionConsumerService`
6. The max length of `name` and `entity_id` fields is 255.

Example request:

```http
POST /api/v1/saml-sp-config HTTP/1.1
Host: onewelcome.example.com
Content-Type: application/json
{
    "name": "My Service Provider",
    "metadata_type": "MANUAL",
    "manual_metadata": {
        "entity_id": "myEntityId",
        "assertion_consumer_service_binding": "urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST",
        "assertion_consumer_service_location": "https://example.sp.com/authn-response",
        "signing_certificate": "-----BEGIN CERTIFICATE----- (...) -----END CERTIFICATE-----",
        "logout_url_post": "https://example.sp.com/logout"
    },
    "user_identifier": "Email",
    "attribute_mappings": {
        "email": "Email address",
        "display_name": "Display name"
    },
    "identity_provider": "oidc_idp"
}
```

Example success response:

```http
HTTP/1.1 201 CREATED
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
Location: /oauth/api/v1/saml-sp-config/60eb60e3-01e0-496e-87f4-fd29737a8881
{
    "id": "60eb60e3-01e0-496e-87f4-fd29737a8881",
    "name": "My Service Provider",
    "entity_id": "myEntityId",
    "metadata_type": "MANUAL"
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

* Endpoint: `/api/v1/saml-sp-config/{service_provider_id}`
* Method: GET

Path parameters:

| Param               | Required | Description                                              |
|---------------------|----------|----------------------------------------------------------|
| service_provider_id | yes      | Unique identifier of the Service Provider configuration. |

Example request:

```http
GET /api/v1/saml-sp-config/120b0682-8ddd-41fc-9ec9-6f76c48afc08 HTTP/1.1
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
    "metadata_type": "MANUAL",
    "manual_metadata": {
        "entity_id": "myEntityId",
        "logout_url_post": "https://example.sp.com/logout",
        "assertion_consumer_service_binding": "urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST",
        "assertion_consumer_service_location": "https://example.sp.com/authn-response",
        "signing_certificate": "-----BEGIN CERTIFICATE----- (...) -----END CERTIFICATE-----"
    },
    "user_identifier": "Email",
    "attribute_mappings": {
        "email": "Email address",
        "display_name": "Display name"
    },
    "identity_provider": "custom",
    "backup_identity_providers": [],
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

* Endpoint: `/api/v1/saml-sp-config/{service_provider_id}`
* Method: PATCH

Path parameters:

| Param               | Required | Description                                              |
|---------------------|----------|----------------------------------------------------------|
| service_provider_id | yes      | Unique identifier of the Service Provider configuration. |

Only the fields that are sent in the request will be changed.

Example request:

```http
PATCH /api/v1/saml-sp-config/60eb60e3-01e0-496e-87f4-fd29737a8881 HTTP/1.1
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

* Endpoint: `/api/v1/saml-sp-config/{service_provider_id}`
* Method: DELETE

Path parameters:

| Param               | Required | Description                                              |
|---------------------|----------|----------------------------------------------------------|
| service_provider_id | yes      | Unique identifier of the Service Provider configuration. |

Example request:

```http
DELETE /api/v1/saml-sp-config/60eb60e3-01e0-496e-87f4-fd29737a8881 HTTP/1.1
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
