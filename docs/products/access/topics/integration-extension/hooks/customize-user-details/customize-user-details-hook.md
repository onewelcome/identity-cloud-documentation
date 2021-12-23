# Customize User Details Web Hook

The user details customization web hook is meant to be identity-centric, meaning that it should only focus on user-related information.
Access control is not its part.

## Web Hook specification

The web hook implementor receives multiple attributes, such as:

- identity of the user
- set of requested scopes
- all available user-related claims

Based on the above information the web hook is allowed to filter, change and introduce new user claims.

Please note that the following claims are considered protected and are not allowed to be modified:

- "iss"
- "aud"
- "verified_claims"
- "sub"
- "urn:onegini.com:oidc:*"
- "group_permissions"

The web hook is executed before the id-token and user-info response creation. This way, we can feed the implementor all information that
are available, and also ensure a deterministic result.

## API Contract

The OpenAPI specification of the web hook can be found [here](./customize-user-details-hook.yaml)

## Web Hook configuration

### Defining a Web Hook instance

In order to define a new web hook instance login to Access's admin console and navigate to `Configuration` -> `System` -> `Web Hooks` tab.
Next, select `Add Web Hook Configuration` option and provide necessary information:

- define the name for the new web hook
- choose `User Details Customization` type
- set the base URL. Please note that the base URL will be concatenated with the web hook path that's defined in the API contract. For
example, by setting `https://hooks.onewelcome.com` the Access seek will be calling the following location `https://hooks.onewelcome.com/v1/customize-token`
- choose the desired authentication scheme

Once done, save the configuration and proceed to your client configuration page.

### Updating client configuration

Due to its characteristics, it only makes sense to configure the user details customization web hook with clients that are acting as OpenID
Connect (OIDC) Relying Parties (RPs). The web hook can be configured for both, dynamic (represented by mobile applications) and static
(represented usually by web apps/portals) clients. Depending on the type of the client you want to configure navigate to `Configuration` ->
`App Configuration` -> `Applications` or `Configuration` -> `Web clients` respectively.
The web hook configuration is available at `OpenID Connect configuration` page that is shown *ONLY* after enabling `openid` scope for that
client.