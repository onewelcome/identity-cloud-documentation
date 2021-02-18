# Creating policy via API
In this guide, we will assume the DABP is available under `https://dabp.onegini.com` url.
The API is protected with OIDC, so you must provide an access token with the request.
Please note that the token must be linked to the account that has "Manage policies" permission.

## How to create a policy
To create a policy a POST request must be sent to `https://dabp.onegini.com/api/v2/policies` with a proper request body
```
{
    "name":"SELL_LIFE_INSURANCE"
}
```


## Example request using curl

```
curl --location --request POST 'https://dabp.onegini.com/api/v2/policies' \
--header 'Content: application/json' \
--header 'Authorization: Bearer  <access token>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name":"SELL_LIFE_INSURANCE"
}'
```


## OpenAPI specification
Here is the detailed description of the create policies endpoint in the openapi format
``` 
"/api/v2/policies": {
      "post": {
        "tags": [
          "Policy API"
        ],
        "summary": "Adds a policy",
        "description": "'POLICY_MANAGE' permission on the root group is required",
        "operationId": "createPolicy",
        "requestBody": {
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/CreatePolicyRequest"
              }
            }
          },
          "required": true
        },
        "responses": {
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
"CreatePolicyRequest": {
        "required": [
          "name"
        ],
        "type": "object",
        "properties": {
          "name": {
            "type": "string",
            "description": "Policy name"
          }
        }
      },

```
