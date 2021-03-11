# ID Token encryption
ID Token encryption gives the ability to provide confidentiality of the claims within the ID Token.

## Enabling encryption and configuring it
Onegini Access must be configured to enable ID Token encryption. The encryption method and jwks endpoint must be configured to use encryption.
You can read more about configuring a [web client with encryption support](../../web-clients/web-client-configuration.md#enabling-openid-connect-capability).

### Choose an encryption method
Following OpenID Connect standards, several different encryption methods for CEK (Content Encryption Key) are supported. This CEK is automatically generated
based on the method chosen and thus the length will vary. Please refer to the [Discovery API](../../../api-reference/oidc/discovery.md) to determine which
encryption methods are supported. These will need to be configured on the web client and also used in your client application when decrypting the ID Token.

### Provide a JWKS endpoint for encryption
Onegini Access supports a remote key set for encrypting the generated CEK. As the relying party, public keys must be shared via this endpoint so the Token
Server can retrieve them in order to encrypt the CEK. Refer to the documentation on [OpenID Connect Encryption](https://openid.net/specs/openid-connect-core-1_0.html#Encryption)
for help with implementation. The endpoint that is served from your application should return a list of JWKs (JSON Web Keys). Refer to the
[RFC specification](https://tools.ietf.org/html/rfc7517.html#section-5) for details on implementation. A proper `max-age` directive should be included with the
`Cache-Control` header as a part of the response. If none is provided, the TTL will default to the `REDIS_DEFAULT_JWKS_URI_RESPONSE_TTL_SECONDS` environment
variable value. Please be sure to consider key rotation as described in the spec.

Onegini Access supports a few different asymmetric algorithms. Please refer to the [Discovery API](../../../api-reference/oidc/discovery.md) for information
on the exact algorithms that are supported.

## Recommended Library
We recommend the [Nimbus JOSE+JWT](https://bitbucket.org/connect2id/nimbus-jose-jwt/wiki/Home) library to help with decryption in your application. 
It contains constants for the encryption methods and algorithms that are supported by our Onegini Access implementation and simplifies the code necessary for
decrypting the JWE and verifying the signed JWT.