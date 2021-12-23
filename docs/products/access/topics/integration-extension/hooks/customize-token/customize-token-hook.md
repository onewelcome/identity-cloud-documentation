# Customize Token Web Hook

The customize token web hook is meant allow access control manipulation. For security reasons, access escalation is not allowed and the
implementor can only reduce the scopes that are associated with the resulting tokens.

## Web Hook specification

The web hook implementor receives multiple attributes, such as:

- identity of the user (if present)
- information about the client who was used to request the token
- set of requested scopes

Based on the above information the web hook is allowed to reduce the scopes.

In case all requested scopes are reduced the Access Service will reject either, the authorization or the token flow.

The web hook is executed before any access grant (authorization code or token) is created. This way, we can ensure the access to potentially
forbidden resources will not be possible, also after refreshing the token.

## API Contract

The OpenAPI specification of the web hook can be found [here](./customize-token-hook.yaml)

## Web Hook configuration

### Defining a Web Hook instance

In order to define a new web hook instance login to Access's admin console and navigate to `Configuration` -> `System` -> `Web Hooks` tab.
Next, select `Add Web Hook Configuration` option and provide necessary information:

- define the name for the new web hook
- choose `Customize Token` type
- set the base URL. Please note that the base URL will be concatenated with the web hook path that's defined in the API contract. For
example, by setting `https://hooks.onewelcome.com` the Access seek will be calling the following location `https://hooks.onewelcome.com/v1/customize-token`
- choose the desired authentication scheme

Once done, save the configuration and proceed to your client configuration page.

### Updating client configuration

The Customize Token web hook can be configured with any OAuth client type. Depending on your choice navigate to `Configuration` ->
`App Configuration` -> `Applications` or `Configuration` -> `Web clients` respectively. The web hook configuration is available at the
`Scopes` section.