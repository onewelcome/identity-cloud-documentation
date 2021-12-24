# Customize Token Web Hook

Customize Token Web Hook enables access control manipulation. The hook allows reducing scopes that are associated with
the grant that gets created as part of the flow in which it gets executed.

To get a better understanding please consider a scenario where the scopes are used to represent active subscriptions. Their validity changes
over time therefore we want to give implementors possibility to actively validate and remove ones that are not available for the given
user/client at a given moment.

## Web Hook specification

The web hook input parameters include:

- identity of the user (if present)
- information about the client who was used to request the token
- set of requested scopes

Based on the above information the web hook is allowed to remove an arbitrary number of the scopes.

In case all requested scopes are removed the Access Service will fail either the authorization or the token flow.

The web hook is executed before any access grant or token is created. This ensures that the access to potentially
forbidden resources is not possible, also after refreshing the token.

## API Contract

The OpenAPI specification of the web hook can be found [here](./customize-token-hook.yaml)

## Web Hook configuration

Before the web hook can be used a few configuration steps are required. Firstly, you need to provide the details about where the web hook API endpoint
can be reached, including the authentication details. Once that's available, you should be able to link the just-defined web hook instance
to your client.

### Defining a Web Hook instance

In order to define a new web hook instance, login to Access's admin console and navigate to `Configuration` -> `System` -> `Web Hooks` tab.
Next, select `Add Web Hook Configuration` option and provide necessary information:

- define the name for the new web hook
- choose `Customize Token` type
- set the base URL. Please note that the base URL will be concatenated with the web hook path that's defined in the API contract. For
example, by setting `https://hooks.onewelcome.com` the Access will be calling the following location
`https://hooks.onewelcome.com/v1/customize-token`
- choose the desired authentication scheme

Once done, save the configuration and proceed to your client configuration page.

### Updating client configuration

Customize Token web hook can be configured with any OAuth client type. Depending on your choice navigate to `Configuration` ->
`App Configuration` -> `Applications` or `Configuration` -> `Web clients` respectively. The web hook configuration is available in the
`Scopes` section.