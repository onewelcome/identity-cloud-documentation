# Refresh Token

A Refresh Token is an opaque token that contains the information required to obtain a new [Access Token](access-token.md). A Refresh Token is long-lived as opposed to an Access Token and ID Token.

A refresh token is an OAuth 2.0 specific token. It is issued as part of the [authorization flow](../../api-reference/description-oauth-endpoint.md#authorization-endpoint). 
However, it is only issued when the client uses the `authorization_code` grant type and if the 'Issue refresh tokens' option is selected in the [web client configuration](../web-clients/web-client-configuration.md). 
For mobile applications refresh tokens are issued by enabling the 'PIN authentication' in the [app configuration](../mobile-apps/app-configuration/app-configuration.md) page. 
Refresh tokens can have a validity specified, so that those are revoked upon usage after certain amount of time.

>**Note:** Please make sure that Refresh Tokens are stored securely and they are not leaked. Leaking refresh tokens basically means that a user account can be compromised.
