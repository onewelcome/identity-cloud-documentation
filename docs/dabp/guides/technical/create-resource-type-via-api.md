# Creating resource type via API

`Resource types` can only be created via API as they should not change after initial setup of the platform.

## How to create a resource type

In this guide, we will assume OneWelcome Identity Cloud is available under the URL: `https://dabp.onegini.com`.
The API is protected with OIDC, so you need to provide an access token to make a successful request.

The access token must be either:

- linked to a person who has the `Add/edit/delete resources and resource types` permission on the root group. The root group is the
  top-level group.
- a machine token with `write` scope. For more information about machine tokens see [non personal requests](non-personal-requests.md)

You can link a resource type to a policy. If you do that, you can only assign resources of that type to groups/members if they also have
this policy assigned. In that sense, a policy on a resource type acts as a filter to whom resources of this type will be available. If you
create a resource type without a linked policy id, then all resources of this type will be available to all groups and members.

To create a resource type a POST request must be sent to `https://dabp.onegini.com/delegation/api/v2/resource-types` with a proper request
body

```json
{
    "name": "Life portfolio type",
    "policyId": <policy id needed to access resources of this type [optional]>,
    "availablePrivileges": ["read", "update"],
    "multiValue": <true|false Information if multiple resources of this type can be assigned to single member of the group [optional]>
}
```

## Example request using curl

```shell
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

```json
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
                "$ref": "#/components/schemas/CreateResourceTypeRequest"
              }
            }
          },
          "required": true
        },
        "responses": {
          "400": {
            "description": "Bad Request",
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

```json
"CreateResourceTypeRequest": {
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
          },
          "multiValue": {
            "type": "boolean",
            "description": "Information if multiple resources of this type can be assigned to single member of the group"
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
