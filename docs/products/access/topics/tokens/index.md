# Tokens

Onegini Access, as the name of the component implies issues tokens. These tokens can be used in all kinds of scenarios. This topic describes what kind of tokens are issued and what's their use.

## Token Types

- JSON Web Tokens (JWT): Tokens that conform to a standard: the [JSON Web Token standard](https://tools.ietf.org/html/rfc7519). A JWT contains information about an identity in the form of claims. They are self-contained. This means that it is not necessary for the party that receives this token to call a server to validate the token or get additional information about the token (read the claims).
- Opaque tokens: Tokens in a proprietary format. Basically the token is a random string. This random string is an identifier to information in a server’s (persistent) storage. To validate an opaque token, the party that receives this token needs to call the issuer, in our case Onegini Access.

## Tokens issued by Onegini Access

Onegini Access issues a number of different tokens each with a different use. All these tokens are issued as part of the OAuth or OpenID Connect specifications

### ID Token

The [ID Token](id-token.md), is a [JSON Web Token (JWT)](https://tools.ietf.org/html/rfc7519) that contains user profile attributes represented in the form of claims. An ID Token is intended for an application or as it is called in Open ID Connect a Relying Party. An ID token contains user information like the user's name, email, and so forth. It is used to verify the users' identity and whether he has correctly authenticated and should get access to the application.

### Access Token

The [Access Token](access-token.md) is a credential that can be used by an application (or OAuth Client / OpenID Connect Relying Party) to access an API.

### Refresh Token

The [Refresh Token](refresh-token.md) is a long-lived token that is used to obtain a new Access Token when a previous one has expired.