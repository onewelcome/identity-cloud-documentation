# Overview

## What is OpenID Connect?
OpenID Connect is a standard that is built on top of Oauth 2.0. It provides support for user authentication, and among others, allow the clients to obtain 
end-user identity attributes in an interoperable manner and manage multiple sessions. It is meant to be a lightweight, modern, JSON-based alternative for SAML.

The OpenID Connect specification defines Relying Party (RP) as an OAuth 2.0 Client requiring End-User authentication, and OpenID Provider (OP) as an OAuth 2.0 
Authentication Server which performs this authentication. As a result of successful authentication, the OpenID Provider issues an ID Token which is
a JWT-compliant JSON object containing a set of [claims](scopes-and-claims.md) - user identity attributes.

Onegini Access acts as an OpenID Provider which manages the ID Tokens and [authentication process](authentication.md). Onegini Access, 
however, does not manage the identity by itself, the actual authentication is delegated to an Identity Provider.

##  Discovery
Onegini Access implements [OpenID Connect Discovery](https://openid.net/specs/openid-connect-discovery-1_0.html) which enables a Relying Party
to determine the capabilities of the OpenID Provider at runtime. See [Discovery API](../../../api-reference/oidc/discovery.md) for the API reference. 

## UserInfo
Onegini Access exposes claims via User Info endpoint. See [User Info](../../../api-reference/oidc/user-info.md) for the API reference.

## Json Web Key Set
Onegini Access exposes keys used for calculating the digital signature of the ID Token via the [JWKS endpoint](../../../api-reference/oidc/jwks.md).

## ID Token encryption (JSON Web Encryption)
The ID Token can be encrypted to hide sensitive claims. By default this feature is turned off. See 
[ID Token encryption](../id-token-encryption/id-token-encryption.md) for more information.

## Session management 
Onegini Access supports session management. See [Session Management](../session-management/index.md) for more details.