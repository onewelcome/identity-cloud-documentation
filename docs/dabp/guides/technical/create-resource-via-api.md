# Creating resource via API
Resources can get created either [through the UI](../functional/manage-resource-via-ui.md) or through the API.
To create a resource you first need to [create a resource type via api](./create-resource-type-via-api.md).
This page is describing how the resource API is used.

## How to create a resource
In this guide, we will assume OneWelcome Identity Cloud is available under the URL: `https://dabp.onegini.com`.
The API is protected with OIDC, so you need to provide an access token to make a successful request.
Please note that the access token must be either:
- linked to a person who has the `Add/edit/delete resource and resource types` permission on the root group. The root group is the top-level group.
- a machine token with `write` scope. For more information about machine tokens see [non personal requests](non-personal-requests.md)

To create a resource a POST request must be sent to `https://dabp.onegini.com/delegation/api/v2/resources` with a proper request body
```
{
    "name":"SELL_LIFE_INSURANCE"
    "resourceTypeId": <id of existing resource type>
}
```

Due to hierarchical nature of resources all new resources will automatically get added to the root group (so they can be assigned to other groups later). 

## Example request using curl

```
curl --location --request POST 'https://dabp.onegini.com/delegation/api/v2/resources' \
--header 'Content: application/json' \
--header 'Authorization: Bearer  <access token>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name":"SELL_LIFE_INSURANCE"
    "resourceTypeId": "6a021283-73fd-430d-b0b7-cd42524dde6f"
}'
```


## OpenAPI specification
Here is the detailed description of the create policies endpoint in the OpenApi format:
``` 
"/delegation/api/v2/resources": {
      "post": {
        "tags": [
          "Resource API"
        ],
        "summary": "Creates resource",
        "description": "Creates resource with a name and a resource type",
        "operationId": "createResource",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateResourceRequest"
              }
            }
          },
          "required": true
        },
        "responses": {
          "405": {
            "description": "Method Not Allowed",
            "content": {
              "*/*": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "403": {
            "description": "Forbidden",
            "content": {
              "*/*": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "404": {
            "description": "Not Found",
            "content": {
              "*/*": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "400": {
            "description": "Invalid 'Onegini-Tenant-ID' header",
            "content": {
              "text/plain": {
                "schema": {
                  "example": "Failed to read 'Onegini-Tenant-ID' header. It should be a proper UUID identifier of a tenant."
                }
              }
            }
          },
          "409": {
            "description": "Conflict",
            "content": {
              "*/*": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "500": {
            "description": "Internal Server Error",
            "content": {
              "*/*": {
                "schema": {
                  "$ref": "#/components/schemas/ErrorResponse"
                }
              }
            }
          },
          "201": {
            "description": "A resource was successfully created",
            "content": {
              "json": {}
            }
          }
        }
      }
    }
```
Schema specification:
```
"CreateResourceRequest": {
        "required": [
          "name",
          "resourceTypeId"
        ],
        "type": "object",
        "properties": {
          "name": {
            "type": "string",
            "description": "Name of the resource"
          },
          "resourceTypeId": {
            "type": "string",
            "description": "ID of the resource type it is attached to",
            "format": "uuid"
          }
        }
      }
```
