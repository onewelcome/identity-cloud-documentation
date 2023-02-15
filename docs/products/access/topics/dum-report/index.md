# Delegated Administration for Business Partners

Delegated Administration for Business Partners ([DABP](../../../../dabp/index.md)) keeps information about group memberships, permissions,
and policies. Applications can use this information to provide fine-grained access to data. Applications can obtain this information via the
OneWelcome's OAuth and OpenID Connect endpoints.

## Person report v2

The group memberships and policies for the user are returned via the claim `urn:onegini.com:oidc:group_policies` in:

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
        "id": "7ca5c304-20d4-4d75-aed7-3239685ac8c2",
        "name": "OneWelcome test group",
        "attributes": [
          {
            "name": "crm_group",
            "value": "OneWelcome test group"
          }
        ],
        "policies": [
          {
            "id": "a25321b1-1416-408d-904d-3d990fc81e52",
            "name": "role_functional_admin"
          },
          {
            "id": "b3992ddd-1049-4097-b676-df656213b97e",
            "name": "role_operator"
          },
          {
            "name": "role_superuser"
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

The [DABP person report](../../../../dabp/guides/technical/person-report.md) is returned via the claim `group_permissions` in:

* JWT access token. It will not be present if the access token will become too large to be used as bearer
  token.
* Token introspection response

The format of this claim is a string. This string can be parsed to a JSON object.

## Configuration

* [Delegated Administration Configuration](dum-engine-configuration.md)
