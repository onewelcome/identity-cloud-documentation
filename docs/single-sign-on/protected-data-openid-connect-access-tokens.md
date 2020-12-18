# Protected data in Single Page Apps

What do the website of a bank, an online retailer, and social media have in common? They serve public facing content and personalized content based on your account. After you have signed in with your bank, you can check your balance, transfer money or request a new (debit) card. At the online retailer, you can check your previous orders or update your wish list. At Twitter, you can place Tweets, receive notifications or update your profile.

These frontends must return the data of the correct users. On this page, you can read how to accomplish that in a Single Page App using Access Tokens obtained via the OpenID Connect standard.

## Sessions

Single Page Apps have more responsibility in the front-end when it comes to handling authentication. With (classic) server side rendered websites, the server detects an unauthenticated request, and it shows a page to sign in. The user then enters their username and password, submits the form, and if the credentials are correct, the server creates a session and returns the session identifier in a cookie. With every next request, the browser sends the cookie, and the server will return the personalized content.

This is still possible when using client side rendering. The server may respond with an HTTP response status code 401 when the user has not authenticated yet. Based on this response, you need to present the sign in form, pass the credentials to the backend, and it returns a session cookie you can use in requests to fetch data.

This classic setup has a few drawbacks. Sessions are usually bound to a server. For larger websites, there are usually several servers that serve the content for the website to guarantee availability and handle the load. Without any extra configuration, the user will switch from one server to the other, but they're not aware of each other's sessions. There are solutions for this problem: either all request from a single user agent are sent to the same server (sticky sessions), or the servers need to synchronize their sessions.  

Another drawback is that the server that's responsible for authentication is also responsible for serving the content. This can work for smaller web applications, but larger websites may require a technical separation on functionality: one web app may be responsible for your savings account, while another web app is responsible for your stock portfolio. As an end-user you expect a single sign-on experience: sign in once, and you won't notice you're switching web apps that form the website of your bank.

## JSON Web Tokens

An alternative for session cookies is using tokens that are sent with every request. Onegini Identity Cloud can hand out these access tokens in two formats: as an opaque token (random, unpredictable strings), or as [JSON Web Tokens](https://en.wikipedia.org/wiki/JSON_Web_Token). A JSON Web Token (JWT, suggested pronunciation "jot") consists of three parts: a header, the payload and a signature.

The payload is a JSON object with so called "claims". Claims can contain information about, for example the validity, the target audience of the token, a user identifier, e-mail address, or subscriptions. An example payload of a JWT:

```json
{
  "sub": "jane.doe",
  "iss": "https://onesurance.onegini.com/oauth",
  "aud": ["Onesurance-backend", "Onesurance-frontend"]
}
```

The payload in the example contains three claims: sub (user), iss (issuer) and aud (audience). Further explanation of these claims will follow. When the frontend requests the profile at the server, it sends the JWT with the request. The backend server validates the JWT and sends the profile of "jane.doe" to the frontend.

### Validation: signature

How can you validate a JWT? The issuer, Onegini Identity Cloud, will sign the header and payload with a JSON Web Key (JWK). By default, it uses a cryptographic algorithm with a private and a public key. The issuer uses the private key to create the signature. The audience (receiver) can use the public key to validate the signature. When the validation passes, the audience can trust the JWT is legit.

In order to validate the signature, the audience needs to know the public key and the algorithm. Onegini Identity Cloud exposes a list of public keys and their algorithms via the `jwks_uri` on `https://<your-tenant-host>/oauth/.well-known/openid-configuration`, currently `https://<your-tenant-host>/oauth/v1/keys`. This list contains the current, the future, and potentially a previously used combination of the key and algorithm. This makes it possible to rotate keys and still be able to validate a JWT that has been issued before. 

### Validation: claims

The payload of a JWT contains claims that you can use to verify its validity:

* `iss` (issuer): a unique identifier for the issuer. You should only trust the JWT when it matches the expected value. The issuer for Onegini Identity Cloud is listed on `https://<your-tenant-host>/oauth/.well-known/openid-configuration`.
* `aud` (audience): list of identifiers that are expected to process this JWT. This list is maintained in the configuration of your application in the Onegini Identity Cloud. Only trust the JWT when your application is listed in the `aud` claim. The audience is a list, because the frontend can pass the JWT access token to the backend which in its turn may even pass it to an internal API. All of these layers can have their own identifier for the audience.
* `sub` (subject): a unique identifier of the user. Not present in tokens that are issued for machine-to-machine authentication. 
* `exp` (expiration time): timestamp till when the JWT is valid. You should not accept the JWT when the current time has passed the expiration.
* `nbf` (not before): timestamp from when the JWT is valid. You should not accept the JWT when the current time is before this value.

The frontend and various backend services can decide independently to which (secured) data the user has access by verifying both the signature, and the claims inside the JWT. By using a JWT access token, they no longer need to synchronize sessions.

## OpenID Connect

How do you obtain a (JWT) access token? There are multiple standards and one of them is [OpenID Connect](https://openid.net/) (OIDC). OpenID Connect is a layer on top of [OAuth 2.0](https://oauth.net/). OAuth is all about authorization (what are you allowed), while OIDC also handles authentication (who are you). For simplicity, we'll treat them as one standard, and use the terminology of OpenID Connect.

![Flow chart of getting data with OpenID Connect](./img/oidc-token.svg)

Your (single page) application is called a Relying Party (RP) in OIDC. It's calling the authentication endpoint in Onegini Identity Cloud, which is an OpenID Provider (OP). We recommend using an existing, [certified library](https://openid.net/certification/#RPs) to handle this. The certified libraries comply to the specifications of the OpenID Foundation for a RP implementation.

When the user reaches the Onegini Identity Cloud, they may need to sign in. How the user authenticates depends on the configuration. Onegini Identity Cloud supports multiple (external) identity providers. When the user has an existing session, it can be reused. After the user has successfully authenticated, the Onegini Identity Cloud sends a short-lived token, called an access grant. Your single page app will exchange this access grant for at most three tokens: an ID token, an access token, and a refresh token. 

### ID token

When your application is configured for and requests at least the `openid` scope, it will receive an ID token. The ID token is a JWT with [claims](https://openid.net/specs/openid-connect-core-1_0.html#Claims) for the time and method of authentication, but also profile data like the user's name, email address, phone number, address or date of birth. Your frontend can directly use the information inside the claims to personalize the response.

### Access token

The access token is meant to access secured data in the backend. The frontend sends the access token with every request to the backend. When it's a JWT, the backend can verify the validity of the access token without calling the Onegini Identity Cloud. The validity, user identifier and scopes of the tokens are claims of the JWT. The backend won't need to create a session, because it will receive the access token with every request.

### Refresh token

Access tokens have a limited validity. For single page apps, we recommend a validity of several minutes, and this is configurable. The validity is send with the token response to the Relying Party. Depending on the configuration, a refresh token is sent with the access token. When the access token is about to expire, or when the access token is refused by the backend, your single page app should use the refresh token to obtain a new set of tokens. This refresh token must be kept in a secure location. The RP must not share the refresh token with backend systems or other sites. Unfortunately a single page app does not have a really secure location to store data in the browser. At the moment, [`sessionStorage`](https://developer.mozilla.org/en/docs/Web/API/Window/sessionStorage) is acceptable to store a refresh token. Be aware, any script with access to your page can read the `sessionStorage`. 

In order to limit the abuse of refresh tokens, we recommend limiting the validity of refresh tokens for Single Page Apps. This validity is configurable in Onegini Identity Cloud and is calculated from the moment the first access token had been issued. When this validity has exceeded, the user needs to authenticate again to obtain a new set of tokens. 

## Responsibilities

When you use a Single Page App, some responsibilities move from the backend to the frontend. The frontend is now responsible for keeping the tokens in a secure enough location and for the user experience when the user has not obtained a token or when tokens have expired.

An advantage of this setup is that multiple (backend) services can handle protected data independently of each other. They no longer need to synchronize sessions for a Single Sign On experience.
