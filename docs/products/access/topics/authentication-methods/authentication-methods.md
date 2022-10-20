# Authentication methods

Clients, depending on their configuration and capabilities, can authenticate with Onegini Access using three different authentication
methods:

* [Client Secret Basic](https://tools.ietf.org/html/rfc6749#section-2.3.1) - uses a pair of a ClientId and a Client Secret that are known
  only to the Authorization Server and the Client itself, as a username and password. These credentials are expected to be sent in a form of
  either:
    * HTTP Basic Authorization Header
    * Url encoded form with client credentials (for HTTP POST requests)
* PKCE - Used by public clients. See the [Proof Key for Code Exchange by OAuth Public Clients](https://tools.ietf.org/html/rfc7636)
  specification.
* [PrivateKeyJWT](https://tools.ietf.org/html/rfc7523) - a method that uses Public Key Infrastructure (PKI) to prove the authenticity of a
  client. The client generates and presents a signed JWT (called `Client Assertion`) and lets the server verify its correctness. Depending
  on the client type (dynamic, static or API) Onegini Access requires the public client key to be provided by the client itself (dynamic) or
  lets you configure it via the admin console (static, API) in the form of JWKs URI or a PEM encoded key (x.509 certificate or public key).
