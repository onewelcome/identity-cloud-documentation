# Delegated User Management

Delegated User Management ([DUM](https://docs-single-tenant.onegini.com/cim/stable/dum-engine/)) keeps information about group memberships and permissions.
Applications can use this information to provide fine grained access to data.

Information about group memberships and permissions are returned in a [JWT access token](../tokens/access-token.md) and in the
[token introspection](../../api-reference/token-introspection.md) response. The format of the response is a string containing the 
[DUM person report](https://docs-single-tenant.onegini.com/cim/stable/dum-engine/topic-guides/person-report.html).

## Configuration

* [DUM Engine Configuration](dum-engine-configuration.md)
* [DUM Engine Configuration API](../../api-reference/admin-api/dum-engine-configuration-api.md)


