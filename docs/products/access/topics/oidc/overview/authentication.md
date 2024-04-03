# OpenID Connect authentication

This page describes the authentication flows to obtain an ID token and/or an access token via OpenID Connect.

## Authorization Code Flow
The Authorization code flow is an extension of the OAuth 2.0 authorization code flow. 

1. The Relying Party (RP) obtains the Authorization Code from the
   [authorization endpoint](../../../api-reference/description-oauth-endpoint.md#authorization-endpoint) using a front-channel (e.g. via a
   web browser).
2. The RP exchanges this code at the [token endpoint](../../../api-reference/description-oauth-endpoint.md#token-endpoint) for an access
   token and/or an ID token. An RP that is capable of storing secrets securely will do this via the back-channel, with an authenticated
   request to the token endpoint. RPs that cannot store secrets securely, e.g. a single page app in the browser, will call the token
   endpoint, and use [Proof Key for Code Exchange](https://tools.ietf.org/html/rfc7636) (PKCE).

In order to obtain the ID Token in this flow, make sure the web client has the grant type `Authorization Code` and the scope `openid` configured. Create an
 OAuth authorization request with the [scope](scopes-and-claims.md) `openid`.

## Implicit Flow

Implicit flow is a one-step flow - the client requests an access token and/or ID Token directly from the authorization endpoint using a
front-channel (e.g. via a web browser). This workflow can be suitable for those clients that cannot store client secret in a secure
fashion (e.g. web applications running inside user agent such as web browser). However, this flow is no longer recommended. Use the
authorization code flow with PKCE.

In order to obtain the ID Token in this flow, make sure the web client has the grant type `Implicit` enabled and create an OAuth
authorization request with `response_type=id_token` and the scope `openid`.

## Hybrid flow

OneWelcome Access does not support the Hybrid flow.

## User interaction

The regular authentication flows assume that they are called when authentication is needed. When the user does not yet have a session with the Identity Provider 
(IdP), the user needs to perform some interaction, such as entering their credentials. When the user does have a session, the IdP redirects them back to the 
OneWelcome Access. This section describes several scenarios to obtain tokens via OpenID Connect.  

### Require login
If the user already has a session with the IdP, the IdP will return the user information to OneWelcome Access automatically. There are situations where
you want to force the user to sign in again to obtain a token. For example, when the user changes personal data, or when another user wants to register for the 
same mobile app on a shared mobile device. Add the following request parameter to the authentication request: `prompt=login`.

### Passive login
Some sites have publicly accessible pages that can show personalized content if the user is authenticated. It's not required to immediately sign in and 
therefore you want to avoid showing a login page until it is needed. You do want to benefit from having an access token and/or ID token if the user is signed in. 
For this case you can use the authorization code flow with a SAML based identity provider.  If the user is signed in at the SAML IdP, and no user interaction is 
required, OneWelcome Access will issue an access token and ID token. 

To perform this passive login, add the following parameter to the authentication request: `prompt=none`.

You can use this passive login for [session management with iframes](../session-management/session-monitoring-with-iframes.md).

### Cookie based login
This is similar to the passive login, but the user does not need to have a valid session with the identity provider. It requires OneWelcome CIM as
identity provider. If the user has a cookie from a previous session in OneWelcome CIM, and no user interaction is required, OneWelcome Access 
will issue an access token and ID token.

To perform the cookie based login, add the following parameters to the authentication request: 
`prompt=none&external_idp=urn:oasis:names:tc:SAML:2.0:ac:classes:PreviousSession`. 
