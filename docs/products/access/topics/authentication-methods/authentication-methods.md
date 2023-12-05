# Client authentication methods

Client authentication allows an OAuth/OIDC client application to prove its identity to the OneWelcome Mobile Identity & Access solution. The simplest way to do this for confidential clients is by using `OAuth Basic Authentication`. Public clients should use `PKCE` as they cannot protect the secret. Companies that require a more secure way of client authentication should be using the `private_key_jwt` method.

## Client Types

* **Confidential clients** are applications that are able to securely authenticate with the authorization server, for example being able to
  keep their registered client secret safe.
* **Public clients** are unable to use registered client secrets, such as applications running in a browser or on the end-user's device.

## Forms of client authentication

### Client Secret Basic

Client Secret Basic uses a pair of a ClientId and a Client Secret that are known only to the Authorization Server and the Client itself as a
username and password. These credentials are expected to be sent in the form of either:

* HTTP Basic Authorization Header
* URL encoded form with client credentials in an HTTP POST body

The [OAuth 2.0 standard (RFC 6749)](https://www.rfc-editor.org/rfc/rfc6749#section-2.3.1) recommends using the request header. Only clients
that are not capable of sending HTTP request headers, should use the HTTP POST body for their credentials.

OAuth 2.0 defines basic authentication as:

`[base64(form-urlencoded(client_id) + : + form-urlencoded(client_secret))]`

An example:

```http
POST /oauth/v1/token HTTP/1.1
Host: token.example.com
Authorization: Basic c2VjcmV0X2FwcDpnYWJpdWdicmVzb2hhZWJob2llcmJnb3dpYWJoYW9oYmE=
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials 
```

### Private key JWT

A [private key JWT](https://tools.ietf.org/html/rfc7523) replaces the client secret in the token request for an asymmetrically signed JWT. This completely removes the use of shared secrets, instead signing the token using a private key that only the client application knows and validating it using a public key that the authorization server knows. This method is again defined as part of OpenID Connect.

An example:

```http
POST /oauth/v1/token HTTP/1.1
Host: token.example.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
&client_id=0862EE05F4602FF63F76D99944816144A2192F73B21B8CCEAD76CC39857CDCAD
&client_assertion_type=urn:ietf:params:oauth:client-assertion-type:jwt-bearer
&client_assertion=eyJhbGciOiJFUzI1NiJ9.ewogICJzdWIiOiAiMDg2MkVFMDVGNDYwMkZGNjNGNzZEOTk5NDQ4MTYxNDRBMjE5MkY3M0IyMUI4Q0NFQUQ3NkNDMzk4NTdDRENBRCIsCiAgImF1ZCI6ICJodHRwczovL2V4YW1wbGUuY29tL29hdXRoIiwKICAiaXNzIjogIjA4NjJFRTA1RjQ2MDJGRjYzRjc2RDk5OTQ0ODE2MTQ0QTIxOTJGNzNCMjFCOENDRUFENzZDQzM5ODU3Q0RDQUQiLAogICJleHAiOiAiMTY4NDExOTQ1OSIsCiAgImlhdCI6ICIxNjc0MTE5NDU5Igp9.VRHgtp7MYaMjgn3LDvDr1Ij3nYrS29eLDFOrbjyhcCBNAqGdObdL3vRI3ZvIeUWe6fQLavzpz55GCj-0Szmfbg
```

If you decode the token, it has the following header and payload:

```json
{
  "alg": "ES256"
}
```

```json
{
  "sub": "0862EE05F4602FF63F76D99944816144A2192F73B21B8CCEAD76CC39857CDCAD",
  "aud": "https://example.com/oauth",
  "iss": "0862EE05F4602FF63F76D99944816144A2192F73B21B8CCEAD76CC39857CDCAD",
  "exp": "1684119459",
  "iat": "1674119459"
}
```

These tokens follow the format defined in [RFC 7523 (JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants)](https://www.rfc-editor.org/rfc/rfc7523). 

#### Process to create a Private key JWT

##### Preparing a payload

Create a JSON formatted payload with the required (and optional) claims. These claims are available:

| Claim | Presence     | Type    | Description                                                                                                                                                                                                                                                      |
|-------|--------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `aud` | **Required** | String  | The audience of the JWT that will use it to validate the authentication. In this case, it is the base URL of the authorization server. For example: _https://example.com/oauth_                                                                                  |
| `exp` | **Required** | Integer | The token expiration time in seconds since January 1, 1970 UTC (UNIX timestamp), for example, `1684119459`. This claim fails the request if the expiration time is more than one hour in the future or if the token is already expired.	                         |
| `iss` | **Required** | String  | This MUST contain the `client_id` of the OAuth Client.                                                                                                                                                                                                           |
| `sub` | **Required** | String  | This MUST contain the `client_id` of the OAuth Client.                                                                                                                                                                                                           |
| `jti` | Depends      | String  | The unique token identifier. If you specify this parameter, the token can only be used once. As a result, subsequent token requests won't succeed. Is **Required** for [OIDC](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) flows. |
| `iat` | Optional     | Integer | When the token was issued in seconds since January 1, 1970 UTC (UNIX timestamp), for example, *1674119459*. If specified, it must be a time before the request is received.	                                                                                     |

This can result in:

```json
{
  "sub": "0862EE05F4602FF63F76D99944816144A2192F73B21B8CCEAD76CC39857CDCAD",
  "aud": "https://example.com/oauth",
  "iss": "0862EE05F4602FF63F76D99944816144A2192F73B21B8CCEAD76CC39857CDCAD",
  "exp": "1684119459",
  "iat": "1674119459"
}
```

##### Preparing a JWK Set (self-signed)

OneWelcome supports Elliptic Curve (min P-256) or RSA (min key length 2048) formats.

A [Java command-line utility](https://github.com/mitreid-connect/json-web-key-generator/releases/download/json-web-key-generator-0.8.2/json-web-key-generator.jar) created by Justin Richer can be used to generate keys in JWK format. 

**Usage**

```shell
java -jar json-web-key-generator.jar -t <keyType> [options]
 -t,--type <arg>           Key Type, one of: RSA, oct, EC, OKP
 -s,--size <arg>           Key Size in bits, required for RSA and oct key
                           types. Must be an integer divisible by 8
 -c,--curve <arg>          Key Curve, required for EC or OKP key type.
                           Must be one of P-256, secp256k1, P-384, P-521
                           for EC keys or one of Ed25519, Ed448, X25519,
                           X448 for OKP keys.
 -u,--usage <arg>          Usage, one of: enc, sig (optional)
 -a,--algorithm <arg>      Algorithm (optional)
 -i,--id <arg>             Key ID (optional), one will be generated if not
                           defined
 -g,--idGenerator <arg>    Key ID generation method (optional). Can be one
                           of: date, timestamp, sha256, sha1, none. If
                           omitted, generator method defaults to
                           'timestamp'.
 -I,--noGenerateId         <deprecated> Don't generate a Key ID.
                           (Deprecated, use '-g none' instead.)
 -p,--showPubKey           Display public key separately (if applicable)
 -S,--keySet               Wrap the generated key in a KeySet
 -o,--output <arg>         Write output to file. Will append to existing
                           KeySet if -S is used. Key material will not be
                           displayed to console.
 -P,--pubKeyOutput <arg>   Write public key to separate file. Will append
                           to existing KeySet if -S is used. Key material
                           will not be displayed to console. '-o/--output'
                           must be declared as well.
 -x,--x509                 Display keys in X509 PEM format
```

**Example**

```shell
java -jar json-web-key-generator.jar -t EC -c P-256 -i 1 -u sig -S -x
```

The result will be similar to this:

```json
{
  "keys": [
    {
      "kty": "EC",
      "d": "0JUmbEAblKfEfvoYyr1b9RtqmqTW_yExZYsBsgHJMko",
      "use": "sig",
      "crv": "P-256",
      "kid": "1",
      "x": "ZsitF5jCfGARMx3dip5b62XY0l6_qQm5NZrOKfHu3CQ",
      "y": "JlIa2T2TVQR2bVFNrjAsxdcsBAi7aPwDp6Dk4cM1CJ4"
    }
  ]
}
```

Together with this public key:

```
-----BEGIN PUBLIC KEY-----
MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEZsitF5jCfGARMx3dip5b62XY0l6/
qQm5NZrOKfHu3CQmUhrZPZNVBHZtUU2uMCzF1ywECLto/AOnoOThwzUIng==
-----END PUBLIC KEY-----
```

The public key should be shared in the Client configuration as a file (`.pem` file) or a `JWKS URI`. Using the `JWKS URI` makes it easier to
roll the keys in the future. The `d` value from the example above should be kept private and not end up in the `JWKS URI`.

##### Generating a JWT

Generate a JWT assertion, including the prepared payload, and sign with the generated **private key**.

#### Making the request to the token endpoint

A client must include the following parameters in a token request when using the `private_key_jwt` method:

| Parameter               | Type   | Description                                                                                             |
|-------------------------|--------|---------------------------------------------------------------------------------------------------------|
| `client_assertion_type` | String | A type of client_assertion. Its value must be `urn:ietf:params:oauth:client-assertion-type:jwt-bearer`. |
| `client_assertion`      | String | The signed JWT that we created in the steps above.                                                      |
| `grant_type`.           | String | Type of the grant used, for example, `client_credentials` or `authorization_code`                       |

#### Making a request to other protected endpoints

[Bearer Token authentication](https://www.rfc-editor.org/rfc/rfc6750) should be used to send the `PrivateKeyJWT` to other endpoints.
The client must send this JWT token in the Authorization header when making requests to protected resources:

```http
Authorization: Bearer <token>
``` 

### PKCE

Client authentication is only useful when a client application can keep a secret. Public clients cannot keep a secret. Examples of those
public client applications are a Single Page Application (SPA) running in the browser, or an app that is installed on the end-user's device.
The same set of credentials would be exposed via the browser or stored on the end-user's device and anyone who has access to the device
would be able to obtain these credentials.

This is typically when you would use Proof Key for Code Exchange, [PKCE](https://tools.ietf.org/html/rfc7636) instead of client credentials
or a private key JWT. PKCE is not used to authenticate the public client, but to relate the initial authorization request to the token
request. PKCE prevents that a malicious
client will obtain a token by intercepting the authorization grant.

You could embed some client credentials with the approach of “Why make it easy for them? It’s another hurdle for the attacker”, but when
it’s the same set of credentials across all instances of that client application, then the benefits are negligible. OneWelcome offers
a mobile SDK for [Android](https://developer.onewelcome.com/android/android-sdk/) and [iOS](https://developer.onewelcome.com/ios/sdk) that
handles dynamic client registration in mobile apps. Each installation of the mobile app will then have its dedicated client credentials.
