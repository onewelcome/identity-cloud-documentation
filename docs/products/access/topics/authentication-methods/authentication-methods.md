# Client authentication methods
Client authentication allows an OAuth/OIDC client application to prove its identity to the OneWelcome Mobile Identity & Access solution. The simplest way to do this for confidential clients is by using `OAuth Basic Authentication`. Public clients should use `PKCE` as they cannot protect the secret. For companies that require a more secure 

## Client Types

* Confidential clients are applications that are able to securely authenticate with the authorization server, for example being able to keep their registered client secret safe.

* Public clients are unable to use registered client secrets, such as applications running in a browser or on a mobile device.

## Forms of client authentication

### Client Secret Basic
[Client Secret Basic](https://tools.ietf.org/html/rfc6749#section-2.3.1) - uses a pair of a ClientId and a Client Secret that are known only to the Authorization Server and the Client itself as a username and password. These credentials are expected to be sent in the form of either:

* HTTP Basic Authorization Header
* Url encoded form with client credentials (for HTTP POST requests)

The [OAuth 2.0 standard (RFC 6749)](https://www.rfc-editor.org/rfc/rfc6749#section-2.3.1) recommends sending this over the request body.

OAuth 2.0 defines basic authentication as:

`[base64(form-urlencoded(client_id) + : + form-urlencoded(client_secret))]`

An example:

```
POST /oauth/v1/token HTTP/1.1
Host: token.customerdomain.com
Authorization: Basic c2VjcmV0X2FwcDpnYWJpdWdicmVzb2hhZWJob2llcmJnb3dpYWJoYW9oYmE=
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials 
```

### PKCE
There is no point in client authentication if a client application cannot keep a secret. Examples of those (public) client applications are a Single Page Application (SPA) running in the browser, and a mobile phone app. The would need to have the plaintext credentials in the end-users browser or on their phone.  This is typically when you would use [PKCE](https://tools.ietf.org/html/rfc7636) without a client secret.

You could embed some client credentials with the approach of “Why make it easy for them? It’s another hurdle for the attacker”, but when it’s the same credentials across all instances of that client application, then the benefits are negligible.


### Private key JWT

A [private key JWT](https://tools.ietf.org/html/rfc7523) replaces the client secret in the token request for an asymmetrically signed JWT. This completely removes the use of shared secrets, instead signing the token using a private key that only the client application knows and validating it using a public key that the authorization server knows. This method is again defined as part of OpenID Connect.

An example:

```
POST /oauth/v1/token HTTP/1.1
Host: token.customerdomain.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
&client_id=0862EE05F4602FF63F76D99944816144A2192F73B21B8CCEAD76CC39857CDCAD
&client_assertion_type=urn:ietf:params:oauth:client-assertion-type:jwt-bearer
&client_assertion=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIwODYyRUUwNUY0NjAyRkY2M0Y3NkQ5OTk0NDgxNjE0NEEyMTkyRjczQjIxQjhDQ0VBRDc2Q0MzOTg1N0NEQ0FEIiwiYXVkIjoiaHR0cHM6Ly9jdXN0b21lcmRvbWFpbi5jb20vb2F1dGgiLCJpc3MiOiIwODYyRUUwNUY0NjAyRkY2M0Y3NkQ5OTk0NDgxNjE0NEEyMTkyRjczQjIxQjhDQ0VBRDc2Q0MzOTg1N0NEQ0FEIiwiZXhwIjoiMTY4NDExOTQ1OSIsImlhdCI6IjE2NzQxMTk0NTkifQ.85phYcg6Jp5md0oOJr65miaSwHRnLmK9iALbNnyHaSg
```

If you decode the token, it has the following header and payload:

```
{
  "alg": "HS256"
}
{
  "sub": "0862EE05F4602FF63F76D99944816144A2192F73B21B8CCEAD76CC39857CDCAD",
  "aud": "https://customerdomain.com/oauth",
  "iss": "0862EE05F4602FF63F76D99944816144A2192F73B21B8CCEAD76CC39857CDCAD",
  "exp": "1684119459",
  "iat": "1674119459"
}
```

These tokens follow the format defined in [RFC 7523 (JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants)](https://www.rfc-editor.org/rfc/rfc7523). 

#### Process to create a Private key JWT

##### Preparing a payload

Create a JSON formatted payload with the required (and optional) claims. These claims are available:

| Claim |	Description               | Type |
|-------|---------------------------|------|
| `aud`   |	**Required**. The URL of the resource that you're trying to access using the JWT to authenticate. For example: _https://customerdomain.com/oauth_ | String |
| `exp`   |	**Required**. The token expiration time in seconds since January 1, 1970 UTC (UNIX timestamp), for example, `1684119459`. This claim fails the request if the expiration time is more than one hour in the future or if the token is already expired.	| Integer |
| `iss`   | **Required**. The issuer of the token. This value must be the same as the `client_id` of the application that you are accessing. |	String |
| `sub`	 | **Required**. The subject of the token. This value must be the same as the `client_id` of the application that you are accessing. | String |
| `jti`   |	**Optional**. The unique token identifier. If you specify this parameter, the token can only be used once. As a result, subsequent token requests won't succeed. | String |
| `iat`   |	**Optional**. When the token was issued in seconds since January 1, 1970 UTC (UNIX timestamp), for example, *1674119459*. If specified, it must be a time before the request is received.	| Integer |

This can result in:

```
{
  "sub": "0862EE05F4602FF63F76D99944816144A2192F73B21B8CCEAD76CC39857CDCAD",
  "aud": "https://customerdomain.com/oauth",
  "iss": "0862EE05F4602FF63F76D99944816144A2192F73B21B8CCEAD76CC39857CDCAD",
  "exp": "1684119459",
  "iat": "1674119459"
}
```

##### Preparing a JWK Set (self-signed)

Currently, OneWelcome only supports EC P-256 keys.

A simple [Java command-line utility](https://github.com/mitreid-connect/json-web-key-generator/releases/download/json-web-key-generator-0.8.2/json-web-key-generator.jar) created by Justin Richer can be used to generate keys in JWK format. 

**Usage**

```
sage: java -jar json-web-key-generator.jar -t <keyType> [options]
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

```
java -jar json-web-key-generator.jar -t EC -c P-256 -i 1 -u sig -S -x
```

The result will be similar to this:
 
```
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

The public key should be shared in the Client configuration as a file (`.pem` file) or a `JWKS URI`. Using the `JWKS URI` makes it easier to roll the keys in the future. The `d` value from the example above should be kept private and not end up in the `JWKS URI`.

##### Generating a JWT

Generate a JWT assertion, including the prepared payload, and sign with the generated **private key**. 

#### Making the request to the token endpoint

A client must include the following parameters in a token request when using the `private_key_jwt` method:

| Parameter               | Description               | Type |
|-------------------------|---------------------------|------|
| `client_assertion_type` | A type of client_assertion. Its value must be `urn:ietf:params:oauth:client-assertion-type:jwt-bearer`. | String |
| `client_assertion`      | The signed JWT that we created in the steps above. |
| `grant_type`.           | Type of the grant used, for example, `client_credentials` |
