# OpenID Connect scopes and claims

In order to issue an ID Token, the authorization request must contain at least the `openid` scope. It is possible to control which user claims are included 
in the ID token by specifying additional scopes.

The table below summarizes the scopes relevant for OpenID Connect.

| scope       | Description                                                                                                                                      |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| openid      | Activates the OpenID functionality and allows to issue ID Token as a part of OAuth 2.0 authorization request.                                    |
| profile     | Requests access to the following claims: `name`, `family_name`, `given_name`, `nickname`, `preferred_username`, `gender`, `birthdate`, `locale`. |
| email       | Requests access to the following claims: `email` and `email_verified`.                                                                           |
| address     | Requests access to `address` claim that is composed of the following fields: `street_address`, `locality`, `region`, `postal_code`, `country`    | 
| phone       | Requests access to the following claims: `phone_number` and `phone_number_verified`                                                              |

The claims are obtained from the Identity Provider via [Person API](https://docs-single-tenant.onewelcome.net/cim/stable/idp/api-reference/person-api.html) 
and mapped as listed below:

| claim                      | Person API source                                                      |
|----------------------------|------------------------------------------------------------------------|
| name                       | profile > name > first + last                                          |
| given_name                 | profile > name > first                                                 |
| family_name                | profile > name > last                                                  |
| nickname                   | profile > name > display_name                                          |
| preferred_username         | profile > name > display_name                                          |
| gender                     | profile > gender                                                       |
| birthdate                  | profile > date_of_birth                                                |
| locale                     | profile > preferred_locale                                             |
| email                      | profile > email_addresses > value                                      |
| email_verified             | profile > email_addresses > verified                                   |
| phone                      | profile > phone_numbers > value                                        |
| phone_number_verified      | profile > phone_numbers > verified                                     |
| address > street_address   | profile > address > street_name + house_number + house_number_addition |
| address > locality         | profile > address > city                                               |
| address > region           | profile > address > region                                             |
| address > postal_code      | profile > address > postal_code                                        |
| address > country          | profile > address > country_name                                       |

## Custom claims

Custom claims that are defined and returned by OneWelcome Access are sharing a constant prefix `urn:onegini.com:oidc` value that allows to uniquely
identify their source and also prevent from potential clashes/collisions.

## Optional claims

### ACR

Via ACR (Authentication Context Class Reference) you request that a specific authentication context must be met upon successful
authentication.

#### CIM

Only supported ACR values will be accepted when using the OneWelcome CIM as the identity provider.
CIM accepts ACR values format containing authentication level and optionally identification level.

Example values:

| ACR value                                                          | Description                                                                                                |
|--------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| urn:onegini.com:oidc:authentication_level:2                        | Requires an authentication level of at least 2 from OneWelcome CIM.                                           |
| urn:onegini.com:oidc:authentication_level:3:identification_level:2 | Requires an authentication level of at least 3 and an identification level of at least 2 from OneWelcome CIM. |

All supported values are exposed via the [Discovery API](../../../api-reference/oidc/discovery.md).

For CIM IdP, only a single ACR value can be specified. Sending multiple values will result in a `Bad Request` error.

Access may return an ACR with a value that is higher than the authentication level that was requested.

#### OAuth/OIDC

For OAuth-based identity providers (e.g. Tulip, Identity Broker), requested ACR values are forwarded upstream to the configured identity
provider.

Supported ACR values from IdP OpenID configuration are exposed via the [Discovery API](../../../api-reference/oidc/discovery.md).
In case of multiple IdPs, the `acr_values_supported` field will contain a superset of ACR values supported by all IdPs.
It means not all IdP types support all ACR values listed in the Discovery endpoint.
Note, that Access does not validate provided ACR values and allows those not in OpenID configuration.
Requested values will be forwarded to the configured IdP and validated by the IdP.

To request a specific ACR value include `acr_values` parameter when requesting ID Token, e.g.:

```http
http://tokenserver.example.com/oauth/authorize?response_type=code&client_id=openid&redirect_uri=http%3A%2F%2F.example.com&scope=openid&state=d5dbda85-ecdb-4172-9ada-7ba15c6982d0&acr_values=urn:onegini.com:oidc:authentication_level:2
```

Further reading:

- [Authentication Level](https://docs-single-tenant.onewelcome.net/cim/stable/idp/authentication/saml/authentication-level.html) explains the
  use-cases for using different `authentication_level`.
- [OpenID Specification](https://openid.net/specs/openid-connect-core-1_0.html#AuthRequest)

### Custom attributes
>**Note:**
> This feature requires the OneWelcome CIM

The OneWelcome CIM supports custom attributes for a user. These attributes are included as claims within ID Token.

### DABP policies

When DABP integration is enabled, OneWelcome Access will attempt to resolve user's policies and group memberships and return them in a form of a 
[custom claim](#custom-claims)`urn:onegini.com:oidc:group_policies` in the id-token. The claim value is a complex JSON object, please refer to the DABP API 
reference to learn more about the object outline.
