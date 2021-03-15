# Creating policies via API
Policies can get created either [through the UI](../functional/manage-policy-via-ui.md) or through the API.
This page is describing how the API is used.

## How to create a policy
In this guide, we will assume Onegini Identity Cloud is available under the URL: `https://dabp.onegini.com`.
The API is protected with OIDC, so you need to provide an access token to make a successful request.
Please note that the access token must be linked to a person who has the `Add/edit/delete policies` permission 
on the root group. The root group is the top-level group.

To create a policy a POST request must be sent to `https://dabp.onegini.com/delegation/api/v2/policies` with a proper request body
```
{
    "name":"SELL_LIFE_INSURANCE"
}
```

All new policies will automatically get added to the root group. 

## Example request using curl

```
curl --location --request POST 'https://dabp.onegini.com/delegation/api/v2/policies' \
--header 'Content: application/json' \
--header 'Authorization: Bearer  <access token>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name":"SELL_LIFE_INSURANCE"
}'
```


## OpenAPI specification
Here is the detailed description of the create policies endpoint in the OpenApi format:
``` 
"/delegation/api/v2/policies": {
  "post": {
    "tags": [
      "Policy API"
    ],
    "summary": "Adds a policy to the system, and links it with the root group",
    "description": "'POLICY_MANAGE' permission on the root group is required",
    "operationId": "createPolicy",
    "requestBody": {
      "content": {
        "application/json": {
          "schema": {
            "$ref": "#/components/schemas/CreateOrUpdatePolicyRequest"
          }
        }
      },
      "required": true
    },
    "responses": {
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
      "201": {
        "description": "A policy was added successfully",
        "content": {
          "json": {
            "schema": {
              "$ref": "#/components/schemas/PolicyDto"
            }
          }
        }
      }
    }
  }
}
```
Schema specification:
```
"CreateOrUpdatePolicyRequest": {
  "required": [
    "name"
  ],
  "type": "object",
  "properties": {
    "name": {
      "type": "string",
      "description": "Policy name"
    }
  },
  "description": "Data to update the policy with"
},
"ErrorResponse": {
  "required": [
    "code",
    "message"
  ],
  "type": "object",
  "properties": {
    "code": {
      "type": "integer",
      "description": "DABP error code",
      "format": "int32"
    },
    "message": {
      "type": "string",
      "description": "Error message"
    },
    "details": {
      "type": "array",
      "description": "Error details",
      "items": {
        "type": "string",
        "description": "Error details"
      }
    }
  }
}
```
