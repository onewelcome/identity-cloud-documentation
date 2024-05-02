# Migration of Web Clients without known plaintext secrets

## Overview

This guide explains how to migrate Web Clients that use Basic Authentication when only the hashed versions of their secrets are available.
We focus on configuring these secrets through our API, assuming the hashing algorithm and its parameters are known.

## Configuring the Web Client with a hshed secret

To configure a Web Client using a hashed secret, utilize the `hashed_client_secret` attribute in the Web Client creation request:

```http
POST /api/v1/configuration/web-clients HTTP/1.1
Host: server.com
Content-Type: application/json
Authorization: Basic <your-credentials>
{
  "name": "my-web-client",
  "client_id": "my-client-id",
  "hashed_client_secret": "$bcrypt$c=12$T/jBeKR12ikAWTPPZ5mj4Q$RUV/BRiDmssw1kAUu9MKWiQ4v2lYOWY"
  ...
}
```

Further details on the Web Client Configuration API can be found on the [Web Clients API](../../api-reference/config-api/web-client.md)
page.

### Supported Hash Formats and Description

Our system adheres to the PHC format, supporting the following secure hashing algorithms:

- **PBKDF2-SHA1**
- **Argon2id**
- **Bcrypt**

#### PHC Format Explanation

The PHC string format is a modular system for specifying password hash parameters in a portable and compact way. It is structured as
follows: `$identifier$params$salt$hash`, where:

- `$identifier` specifies the hashing algorithm (e.g., bcrypt, pbkdf2-sha1, argon2id).
- `params` defines operational parameters such as cost or iterations.
- `salt` is the salt used in hashing, encoded in base64.
- `hash` is the actual hash result, also encoded in base64.

Examples of PHC Formatted Hashes for each supported algorithm:

- **PBKDF2-SHA1**: `$pbkdf2-sha1$i=10000$test$E3B0M7MEBhwTsFDZAIA7hWQ2Zpc=`
- **Argon2id**: `$argon2id$i=2,m=65536,p=1$dGVzdA==$otNQ21ttnzeFdwPncWePGZpLhNp6Tyss/r0RU3G+9sY=`
- **Bcrypt**: `$bcrypt$c=12$T/jBeKR12ikAWTPPZ5mj4Q$RUV/BRiDmssw1kAUu9MKWiQ4v2lYOWY`

#### Converting bcrypt to PHC Format

If your secret is stored in bcrypt format but not in a PHC-compliant structure, you may need to convert it. This conversion ensures
compatibility with our system. Below is a Python script that demonstrates how to perform this conversion:

```python
#!/usr/bin/env python3
import sys


def bcrypt_to_phc(bcrypt):
    _, _, cost, salt_hash = bcrypt.split('$')
    salt, hash = salt_hash[:22], salt_hash[22:]
    translation_table = str.maketrans(
        "./ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789",
        "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"
    )
    salt_standard_b64 = salt.translate(translation_table)
    checksum_standard_b64 = hash.translate(translation_table)
    return f"$bcrypt$c={cost}${salt_standard_b64}${checksum_standard_b64}"


print(bcrypt_to_phc(sys.argv[1]))
```

To convert a bcrypt hash to PHC format, run the script with the bcrypt hash as an argument:

```bash
$ ./bcrypt.py '$2a$12$R9h/cIPz0gi.URNNX3kh2OPST9/PgBkqquzi.Ss7KIUgO2t0jWMUW'
```

The output will be a PHC-formatted bcrypt hash:

```plaintext
$bcrypt$c=12$T/jBeKR12ikAWTPPZ5mj4Q$RUV/BRiDmssw1kAUu9MKWiQ4v2lYOWY
```
