# Web client configuration

Web clients are often websites or portals that run on a server and use OAuth for authentication. They need to be configured via the admin console. This section
describes the steps related to configuration of these web clients.

## Creating a Web Client

In order to create a new web client, Go to the `Configuration` tab of the administration console and click the `Web Clients` tab.
On the overview that is shown you can see all the existing web clients.

>**Note:**
>  **Web clients** are meant for web servers. In this case you or a trusted party is managing the hardware on which this web client runs.
We also reference these as static OAuth clients.

To add a new web client, click on the add button. The following form will appear:

![Create Web Client](img/add-web-client.png)

Fill all the mandatory fields (marked with `*` on the form). The other fields are optional.

| Field                             | Required | Example value       | Details
|-----------------------------------|----------|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| Name                              | yes      | My demo portal      | Display name of this web client. This is only used for informational purposes in the Admin console and APIs.
| Client ID                         | yes      | F167433E63CE8BD877… | Unique identifier of this web client.
| Authentication method             | yes      | Client secret       | Options are `Client Secret` for confidential clients using secret, `Private key JWT` for confidential clients using self signed client assertions or`PKCE` [Proof Key for Code Exchange](https://tools.ietf.org/html/rfc7636/) for public clients. `Client secret` requires to enter a predefined secret in this configuration. PKCE is only available for the grant type `Authorization code`.
| Client secret                     | depends  | AF33E2BF29C54A4639… | Secret to get an Access token. Required when the Authentication method is `Client secret`.
| Dynamic JWT Configuration         | depends  | http://site.com/jwks| JWKS uri with public keys for private key JWT authentication.  Required when authentication method is `Private key JWT` and dynamic JWKS source is chosen.
| Static JWT Configuration          | depends  |                     | Plain public key for private key JWT authentication. Required when authentication method is `Private key JWT` and Plain public key JWKS source is chosen.
| Grant types                       | yes      | Authorization code  | Supported OAuth grant types for a web client. The available [grant types](#grant-types) are explained in a section below.
| Access token format               | yes      |                     | Defines whether the [Access token](../tokens/access-token.md) is returned as JSON Web Token (JWT) or as Opaque (random) string. The JWT access token contains the user identifier for grant type `Authorization code`.
| Redirect URL                      | depends  |                     | This is the full URI to which the end-user is redirected after they have successfully logged in and given authorization for the requested scopes. Required for grant type `Authorization code`.
| Additional redirect URLs          | no       |                     | Additional URIs to which the end-user can be redirected. See "Redirect URL".
| Access grant validity             | depends  | 30                  | Number of seconds an access grant is valid. This is the time a client has to exchange an authorization grant (a.k.a access grant) for an access token. In a common scenario this is done in at most a couple of seconds. Required for grant type `Authorization code`.
| Access token validity             | depends  | 3600                | Number of seconds an access token is valid. An access token is the temporary key with which a client can fetch resources. When this period has expired the access token has to be refreshed using a refresh token. Required for grant types `Authorization code` and `Client credentials`.
| Issue refresh token               | no       |                     | If this is enabled a refresh token will be issued in case of the authorization code grant type is used. This refresh token can be used by the web client to issue a new access token.
| Refresh token validity            | no       | 7200                | Number of seconds an refresh token is valid. Available when `Issue refresh token` is set to true.
| Allow simultaneous sessions       | no       | true                | If true, it will allow multiple sessions for the same client, user, and scope to exist at the same time. There are security implications as tokens will exist even after the same credentials are used to log in again. If setting is off, previous sessions are cleaned up when a new one is established.
| Maximum simultaneous sessions     | no       | 5                   | This limits the number of simultaneous sessions when this is enabled. The oldest session is invalidated when the user exceeds the limit. Value must be between 2 and 25 (inclusive).
| Audience: Resource gateways       | no       |                     | The [Resource gateways](../general-app-config/resource-gateway/resource-gateway.md) that validate the access tokens. It is used to populate the `aud` claim in a JWT access token.
| Delegated Administration configuration      | no       | myDabpConfig         | Configuration of Delegated Administration that should be used to gather person report containing information about group memberships and permissions.
| Group permissions version         | no       |                     | By default, the latest version of the DABP person report is returned in claims. Change the default to legacy if your application depends on the legacy version 1 of this report.
| Additional audiences              | no       | aud2, https://example.com | A set of case sensitive strings that identifies the audience of the issued access token besides the resource gateways. It is used to populate the `aud` claim in a JWT access token.
| Default scopes                    | no       |                     | The scopes that are assigned to an authorization request if no scopes are requested. If no scopes are requested by the web client in the authorization request and no defaults specified Onegini Access will fail the request.
| Additional scopes                 | no       |                     | The scopes the web client is allowed to request apart from the default scopes. The default scopes are also counted as scopes that can be requested by a web client.
| Identity provider                 | depends  | myIdp               | The identity provider used when a user authenticates for this web client. See the [Identity Provider configuration topic](../general-app-config/identity-providers/identity-providers.md).
| Additional Identity provider(s)   | no       | addIdp1, addIdp2    | Additional identity providers that can be used when a user authenticates with this application. A primary must be specified if additional are entered.
| Logo URI                          | no       |                     | URI of the logo for this web client, it can be used in for example the consent screen.
| Public base URI                   | no       |                     | When the client has the deprecated Grant type `Validate access token`, it has some URI where it can be reached on. This should be the URI that all paths for calls to this client should start with.
| Template set                      | no       | payment-app         | Every web client can have its own branding. Therefore multiple sets of templates are supported. The prefix corresponds to the directory in which a set of templates is stored.
| Skip Consent page                 | no       | true                | In the OAuth flow consent is requested because a web client requests access to the user's data. When the web client is managed by a third party you may want to notify the user that this web client will have access to his personal information. Which information an web client has access to is defined by the requested scopes. When consent is disabled the user is not required to give consent for the web client during the authorization process.


Additional information about the configuration of [Scopes](../general-app-config/scopes/scopes.md), [Identity providers](../general-app-config/identity-providers/identity-providers.md) and [Authentication methods](../authentication-methods/authentication-methods.md) can be found in separate chapters.

In order to enable the OpenID Connect capability, add the scope `openid` either to `Default scopes` or `Additional scopes`. This activates a second step of the Web Client config:  [OpenID Relying Party configuration](../oidc/configuration/configuration.md#openid-connect-relying-party-configuration).

<span id="grant-types"></span>
### Description of grant types
Grant types can be configured for a web client.

| Grant type            | Description
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------
| Authorization code    | Specifies whether the OAuth client can use the OAuth authorization code grant type in order to allow this web client to request an access token on behalf of the end-user. This is the only grant type that is allowed for public clients using PKCE as authentication method.
| Client credentials    | Specifies whether a web client can use its client credentials to request an access token. Note that this access token is not linked to a user since it's solely requested by the web client without any user interaction. This function is typically used for machine-to-machine communication.
| Validate access token | This Grant type has been deprecated. This allows a Web client to validate an Access token and get additional information about it. See the topic on [configuration of a resource gateway](../general-app-config/resource-gateway/resource-gateway.md).

## Removing a web client

In order to remove a web client, Go to the `Configuration` tab of the administration console and click the `Web Clients` tab. On the list of web client find
the one you want to remove and click the delete button on the right (You will be asked to confirm you decision).
