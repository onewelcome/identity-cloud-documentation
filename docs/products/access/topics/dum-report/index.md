# Delegated Administration for Business Partners

Delegated Administration for Business Partners ([DABP](../../../../dabp/index.md)) keeps information about group memberships, permissions, resources,
and policies. Applications can use this information to provide fine-grained access to data. Applications can obtain this information via the
OneWelcome's OAuth and OpenID Connect endpoints.

## Person report v2

The [DABP person report](../../../../dabp/guides/technical/person-report.md) for the user is returned via the
claim `urn:onegini.com:oidc:group_policies` in:

* [JWT access token](../tokens/access-token.md). It will not be present if the access token will become too large to be used as bearer
  token.
* [ID token](../tokens/id-token.md)
* [Userinfo](../../api-reference/oidc/user-info.md) response
* [Token introspection](../../api-reference/token-introspection.md) response

The format of this claim is a JSON object.

Example of this claim:

```json
{
  "urn:onegini.com:oidc:group_policies": {
    "groups": [
      {
        "id": "2374b2db-e690-4f3a-89e0-ccd5aaf6c601",
        "name": "Intermediary A",
        "policies": [
          {
            "id": "77195a4e-c610-4716-b790-721a5fdde1e6",
            "name": "Pension"
          },
          {
            "id": "6fbe9789-ee84-46eb-9234-2d2d711a0328",
            "name": "Insurance"
          }
        ],
        "attributes": [
          {
            "name": "salesforceId",
            "value": "12345"
          }
        ]
      },
      {
        "id": "2374b2db-e690-4f3a-89e0-ccd5aaf6c601",
        "name": "Intermediary B",
        "policies": [
          {
            "name": "role_superuser"
          },
          {
            "id": "77195a4e-c610-4716-b790-721a5fdde1e6",
            "name": "Pension"
          },
          {
            "id": "6fbe9789-ee84-46eb-9234-2d2d711a0328",
            "name": "Insurance"
          }
        ],
        "attributes": [],
        "resources": [
          {
            "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
            "name": "string",
            "externalId": "123456",
            "privilege": "read",
            "resourceType": {
              "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
              "name": "home insurance"
            }
          }
        ]
      }
    ]
  }
}
```

## Legacy person report v1

!!! Warning
This format should not be used for new implementations.

The legacy DABP person report is returned via the claim `group_permissions` in:

* JWT access token. It will not be present if the access token will become too large to be used as bearer
  token.
* Token introspection response

The format of this claim is a string. This string can be parsed to a JSON object.

Example of this claim:

```json
{
  "group_permissions": "{\"person\":{\"idp_type\":\"CIM\",\"person_id\":\"8e272e60-2f67-4737-9212-f4f13e6b01eb\",\"first_name\":\"John\",\"last_name\":\"Smith\"},\"group_permissions\":[{\"id\":\"f530f30d-8322-47d1-93ff-19f4fea37c79\",\"permissions\":[\"GROUP_MANAGE\",\"GROUP_POLICY_MANAGE\",\"PERMISSION_MANAGE\"],\"custom_attributes\":{}}],\"policies\":[{\"id\":\"160d8767-b7ea-4706-9738-4dc221658868\",\"name\":\"Organisation policy\",\"scopes\":[\"145fabf0-8b05-4a4d-be15-95c7b568da63\",\"fe987430-8f0f-4fe6-948c-3d77cf05279a\",\"7b10aea0-6ca5-40f6-8fd5-acd060dfad95\"],\"subject\":{\"type\":\"PERSON\",\"subject_id\":\"13db83a6-bb3f-493a-b614-e86a404c2142\"}}]}"
}
```

## Configuration

* [Delegated Administration Configuration](dum-engine-configuration.md)
