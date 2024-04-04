# Resource Owner Password Credentials

!!! Warning
    The Resource Owner Password Credentials (ROPC) is deprecated and is no longer considered secure for most scenarios. It directly handles
    usernames and passwords, which could increase the risk of security vulnerabilities. We strongly recommend migrating
    to [Custom Registration](../custom-registration/index.md) flows, which provide enhanced security features and better align with
    current best practices in identity management. Additionally, it's imperative to restrict the use of the ROPC flow solely to private
    clients capable of securely holding a secret. Failure to protect this secret renders the token endpoint vulnerable to credential
    stuffing attacks.

The `Resource owner password credentials` grant type cannot be chosen when either `Authorization code` or `Device code` is configured and
vice versa.

Features that require user interaction via the browser are not supported for web clients using the ROPC. So for example consent
and additional user authentication (SMS) are not available.

The ROPC feature works in combination
with [SAML ECP PAOS binding](https://docs.oasis-open.org/security/saml/Post2.0/saml-ecp/v2.0/cs01/saml-ecp-v2.0-cs01.pdf). Therefore a web
client using this feature should have
a [SAML identity provider configured](../general-app-config/identity-providers/identity-providers.md#configure-a-saml-identity-provider).
The configured SAML identity provider requires a single sign on service with a `urn:oasis:names:tc:SAML:2.0:bindings:SOAP` binding in its
metadata. Attribute mappings of the identity provider will be used to set the user id and other user properties.

The [RFC](https://tools.ietf.org/html/rfc6749#section-4.3.2) specifies that the authorization server should protect against brute force
attacks. For this protection the OneWelcome Access relies on the used identity provider.

When a [scope verification service](../integration-extension/scope-verification/scope-verification.md) is configured, requested scopes will
be verified. In case of a verification failure a `400 Bad request` response with `unauthorized_user` error is returned. This error response
contains a `error_uri` field containing the scope validation failed uri configured for this [scope](../general-app-config/scopes/scopes.md).


For other error responses please refer to the [RFC](https://tools.ietf.org/html/rfc6749#section-4.3). 
