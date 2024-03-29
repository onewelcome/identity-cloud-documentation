# OAuth 2.0 and OpenID Connect Overview

To decide which authentication flow is best for you based on the type of application that you are building, you first need to understand OAuth 2.0 and OpenID Connect and how you can implement these two flows using the OneWelcome Identity Cloud.

## OAuth 2.0 vs OpenID Connect

- The [OAuth 2.0](https://tools.ietf.org/html/rfc6749) protocol controls authorization to access a protected resource, like your web app, native app, or API service.
- The [OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html) protocol is built on the OAuth 2.0 protocol and helps authenticate users and convey information about them. It is also more strictly compared to plain OAuth 2.0, for example in its scope definitions.

The OAuth 2.0 protocol provides API security through scoped access tokens. OAuth 2.0 enables you to delegate authorization, while the OpenID Connect protocol enables you to retrieve and store authentication information about your end users. OpenID Connect extends OAuth 2.0 by providing user authentication and single sign-on (SSO), and single logout (SLO) functionality.

### OAuth 2.0

OAuth 2.0 is a standard that apps use to provide client applications with access. If you would like to grant access to your application data in a secure way, then you want to use the OAuth 2.0 protocol.

The OAuth 2.0 spec has four important roles:

- The "*authorization server*" — The server that issues the access token. In this case the OneWelcome Identity Cloud is the authorization server.
- The "*resource owner*" — Normally your application's end user that grants permission to access the resource server with an access token
- The "*client*" — The application that requests the access token and then passes it to the resource server
- The "*resource server*" — Accepts the access token and must verify that it's valid. In this case this is your application back-end.

Other important terms:

- An OAuth 2.0 "*grant*" is the authorization given (or "granted") to the client by the user. Examples of grants are "authorization code" and "client credentials". Each OAuth grant has a corresponding flow, explained below.
- The "*access token*" is issued by the authorization server (OneWelcome Identity Cloud) in exchange for the grant.
- The "*refresh token*" is an optional token that is exchanged for a new access token if the access token has expired.

The usual OAuth 2.0 grant flow looks like this:

1. Client requests authorization from the resource owner (usually the user).
2. If the user gives authorization, the client passes the authorization grant to the authorization server (OneWelcome Identity Cloud).
3. If the grant is valid, the authorization server returns an access token, possibly alongside a refresh and/or ID token.
4. The client now uses that access token to access the resource server.

> **Note:** For a deeper dive into OAuth 2.0, see the [OAuth 2.0 spec](https://tools.ietf.org/html/rfc6749).

At the core of both OAuth 2.0 and its OpenID Connect extension is the authorization server. Each authorization server has a unique issuer URI and its own signing key for tokens to keep a proper boundary between security domains. In the context of this guide, The OneWelcome Identity Cloud is your authorization server.

The authorization server also acts as an OpenID Connect Provider, which means you can request [ID tokens](https://openid.net/specs/openid-connect-core-1_0.html#IDToken) in addition to [access tokens](https://tools.ietf.org/html/rfc6749#section-1.4) from the authorization server endpoints.

### OpenID Connect

OpenID Connect is an authentication standard built on top of OAuth 2.0. It adds an additional token called an ID token. OpenID Connect also standardizes areas that OAuth 2.0 leaves up to choice, such as scopes, endpoint discovery, and dynamic registration of clients. OneWelcome is [OpenID Certified](https://openid.net/certification/).

Although OpenID Connect is built on top of OAuth 2.0, the [OpenID Connect specification](https://openid.net/connect/) uses somewhat different terms for the roles in the flows:

- The "*OpenID provider*" — The authorization server that issues the ID token. In this case the OneWelcome Identity Cloud is the OpenID provider.
- The "*end user*" — Whose information is contained in the ID token
- The "*relying party*" — The client application that requests the ID token
- The "*ID token*" is issued by the OpenID Provider and contains information about the end user in the form of claims.
- A "*claim*" is a piece of information about the end user.

The high-level flow looks the same for both OpenID Connect and regular OAuth 2.0 flows. The primary difference is that an OpenID Connect flow provides an ID token, in addition to any access or refresh tokens.

## Choosing an OAuth 2.0 flow

Which OAuth flow that you use depends on your use case. The table below maps application types to our recommended OAuth 2.0 flows. If you'd like more information, keep reading for help with choosing an OAuth flow based on (1) the type of token that you need, and/or (2) the type of client application that you are building.

### Recommended flow by application type

The table shows you which OAuth 2.0 flow to use for the type of application that you are building:

| Type of Application     | OAuth 2.0 Flow                                          |
| :---------------------- | :------------------------------------------------------ |
| Server-side (AKA Web)   | Authorization Code Flow                                 |
| Single-Page Application | Authorization Code Flow with PKCE                       |
| Mobile apps             | OneWelcome Mobile SDK or Authorization Code Flow with PKCE |
| Service                 | Client Credentials                                      |

### Does your application need an ID token?

Any OAuth flow will give you an access token, but not all flows are supported by OIDC and therefore do not supply an ID token.

|                                  | Access Token | ID Token |
| :------------------------------- | :----------: | :------: |
| **Authorization Code**           |      ✅       |    ✅     |
| **Authorization Code with PKCE** |      ✅       |    ✅     |
| **Client Credentials**           |      ✅       |    ❌     |

### Authorization Code Flow

The Authorization Code flow is best used by server-side apps. The apps should be server-side because the request that exchanges the authorization code for a token requires a client secret (or JSON Web Key Set), which has to be stored in your client. The server-side app requires an end user, however, because it relies on interaction with the end user's web browser, which redirects the user and then receives the authorization code.

```mermaid
sequenceDiagram
    participant RO as Resource Owner (User)
    participant RP as Web application
    participant AS as Authorization Server (OneWelcome)
    participant RS as Resource Server (Your App)

    RP->>AS: Authentication Code Request to /authorize
    AS->>RO: 302 redirect to authentication prompt
    RO->>AS: Authentication & Consent
    AS->>RP: Authentication Code Response
    RP->>AS: Send auhorization code + client secre (or JWKS) to /token
    AS->>RP: Access token, Refresh token, and ID token
    RP->>RS: Request with Access token
    RS->>RP: Response
```

### Authorization Code Flow with PKCE

For web/native/mobile applications, the client secret can't be stored in the application because it could easily be exposed. Additionally, mobile redirects use `app://` protocols, which are prone to interception. Basically, a rogue application could intercept the authorization code as it is being passed through the mobile/native operating system. Therefore native apps should make use of Proof Key for Code Exchange (PKCE), which acts like a secret but isn't hard-coded, to keep the Authorization Code flow secure.

[PKCE](https://oauth.net/2/pkce/) is an extension to the regular Authorization Code flow, so the flow is very similar, except that PKCE elements are included at various steps in the flow.

> **Note:** The Authorization Code Flow with PKCE doesn't support refresh tokens for SPAs and other browser-based apps.

The PKCE-enhanced Authorization Code flow requires your application to generate a cryptographically random key called a "code_verifier". A "code_challenge" is then created from the verifier, and this challenge is passed along with the request for the authorization code.

When the authorization code is sent in the access token request, the code verifier is sent as part of the request. The authorization server recomputes the challenge from the verifier using an agreed-upon hash algorithm and then compares that. If the two code challenges and verifier match, then it knows that both requests were sent by the same client.

A rogue app could only intercept the authorization code, but it wouldn't have access to the code challenge or verifier, since they are both sent over HTTPS.

```mermaid
sequenceDiagram
    participant RO as Resource Owner (User)
    participant RP as Web application
    participant AS as Authorization Server (OneWelcome)
    participant RS as Resource Server (Your App)

    RP->>RP: Generate PKCE code_verifier & code_challenge
    RP->>AS: Authentication Code Request + code_challenger to /authorize
    AS->>RO: 302 redirect to authentication prompt
    RO->>AS: Authentication & Consent
    AS->>RP: Authentication Code Response
    RP->>AS: Send auhorization code + code_verifier to /token
    AS->>AS: Checks PKCE code
    AS->>RP: Access token, Refresh token, and ID token
    RP->>RS: Request with Access token
    RS->>RP: Response
```

### Client Credentials Flow

The Client Credentials flow is intended for server-side (AKA "confidential") client applications with no end user, which normally describes machine-to-machine communication. The application must be server-side because it must be trusted with the client secret, and since the credentials are hard-coded, it can't be used by an actual end user. It involves a single, authenticated request to the `/token` endpoint, which returns an access token.

> **Note:** The Client Credentials Flow doesn't support refresh tokens.

```mermaid
sequenceDiagram
    participant RO as Client + Resource Owner
    participant AS as Authorization Server (OneWelcome)
    participant RS as Resource Server (Your App)

    RO->>AS: Acces token request to /token
    AS->>RO: Access token
    RP->>RS: Request with Access token
    RS->>RO: Response
```
