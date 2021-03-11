# Delegated Administration for Business Partners

Delegated Administration for Business Partners ([DABP](../../../../dabp/index.md)) keeps information about group memberships and permissions.
Applications can use this information to provide fine-grained access to data.

Information about group memberships and permissions are returned in a [JWT access token](../tokens/access-token.md) and in the
[token introspection](../../api-reference/token-introspection.md) response. The format of the response is a string containing the 
[DABP person report](../../../../dabp/guides/person-report.md).

## Configuration

* [Delegated Administration Configuration](dum-engine-configuration.md)
