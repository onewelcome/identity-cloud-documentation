# Creating resource type via API
Resource types can be created only via API as they should not change after initial setup of the platform.

## How to create a resource type
In this guide, we will assume Onegini Identity Cloud is available under the URL: `https://dabp.onegini.com`.
The API is protected with OIDC, so you need to provide an access token to make a successful request.
Please note that the access token must be linked to a person who has the `Add/edit/delete resources and resource types` permission 
on the root group. The root group is the top-level group.

Resource type can be linked to a policy. If a resource type is linked to policy, then all resources of that type can be assigned only to groups/members if they also have this policy assigned.
You can think that a policy on a resource type acts as a filter to whom a resources of this type will be available.
If resource type is created without a policy id, then all resources of this type will be available to all groups and members 

To create a resource type a POST request must be sent to `https://dabp.onegini.com/delegation/api/v2/resource-types` with a proper request body
```
{
    "name": "Life portfolio type",
    "policyId": <policy id required to access resources of this type [optional]>
    "availablePrivileges": ["read", "update"]
}
```

## Example request using curl

```
curl --location --request POST 'http://dabp.onegini.com/delegation/api/v2/resource-types' \
--header 'Authorization: Bearer <access token>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "General portfolio",
    "availablePrivileges": ["read", "update"]
}
```


## OpenAPI specification
Here is the detailed description of the create policies endpoint in the OpenApi format:
``` 
"/delegation/api/v2/resource-types": {
      "post": {
        "tags": [
          "ResourceType API"
        ],
        "summary": "Creates resource types",
        "description": "Creates resource types with a list of connected privileges",
        "operationId": "createResourceType",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CreateOrUpdateResourceTypeRequest"
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
            "description": "A resource type was successfully created",
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
"CreateOrUpdateResourceTypeRequest": {
        "required": [
          "name"
        ],
        "type": "object",
        "properties": {
          "name": {
            "type": "string",
            "description": "Name of the resource type"
          },
          "policyId": {
            "type": "string",
            "description": "ID of the policy it is attached to",
            "format": "uuid"
          },
          "availablePrivileges": {
            "type": "array",
            "description": "List of attached privileges",
            "items": {
              "$ref": "#/components/schemas/CreatePrivilegeDto"
            }
          }
        }
      },
      "CreatePrivilegeDto": {
        "type": "object",
        "properties": {
          "name": {
            "type": "string"
          }
        },
        "description": "List of attached privileges"
      }
```
