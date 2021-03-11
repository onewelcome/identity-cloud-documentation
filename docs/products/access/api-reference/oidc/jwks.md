# Json Web Key Set API

This API exposes the keys used to sign the ID Token in the format of JWKS as described in [RFC7517](https://tools.ietf.org/html/rfc7517).
It allows the client to verify the signature of the ID Token in a key rotation scenario as explained in [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0-final.html#RotateSigKeys).

Endpoint: `GET /oauth/v1/keys`

**Example response**
```json
{
    "keys": [
        {
            "kty": "RSA",
            "e": "AQAB",
            "use": "sig",
            "kid": "db5aac2e-504b-4845-8900-4d64d2afd3ad",
            "alg": "RS256",
            "n": "kVi9JJy_KX3xMhV2Lo07z62HWpDTpJ1HIYr7QzOxPiOq1PsR8vZEXjZd5phJFsPFTUxLP53WXF3Lrqw2CjY7OEEmt3SlTIS1EfWCxPSRgLN_5GKyVFpfjhTCzwJFZPsI3cU4WC5qPKpyEpXqZgWflrMjrVv98Dy45gQe8n5_NTrjqWfI-sz2NQIxe0d7Y61GT_1TsbPjb7Gqe1yz1f8owsEgAlFjxxbCIDAZpQ-dxYZnoFiMeHqt8ybUZEefuTj6kfoK9KxabpJ4jCwMWKH05dQRRn9dRp10cT1cZILzZdPzlPKnQPSIT3QIXcUeT_m4b-CFHn37B0RwJGFF5MpyZw"
        },
        {
            "kty": "RSA",
            "e": "AQAB",
            "use": "sig",
            "kid": "59b04937-7a12-4ff7-9d27-67b55637da9f",
            "alg": "RS256",
            "n": "rd6AzIlRyfIx86AVqb0RcqMmWOUNDssbSyf1LsWKIS-eubUpPaJvTTggriEDxf2aMpmpIeDY-ZBQuiN5K5DuvW3NLg73SFRWlMafWQh9Mta9pJPqt_XpvQEKXPmjSHkIVn16iaymiVuYALac4dGCuUqNTuz__Kle2lb_7SxxiE_c_ArbsQRNmjI7QXu06B9LneVVDCPTYXZ0Oc_ISOD4ilLOcUEMNLM8KmZHZGH3S6QRNCbIrFjZA5VKWf65IX3uPi2kKJ79UGNp7NWDaqXxs8MQqiAmC4qDjNfkXzbJDigoSbaOpeUTQ2IndiouV7ugT-SF7aCUe0VXXknEnuD3qQ"
        },
        {
            "kty": "EC",
            "use": "sig",
            "crv": "P-256",
            "kid": "a1328084-d75d-4ea7-a734-fe5257a5faff",
            "x": "Nv9EjLjxRYXpb-Ld5i2ELEx9iOn6X54HkzlptLHe0tM",
            "y": "Wo2S-WE9A-sOY3NHjyhoeXrzm1Yr5CioOqsoXaL7NEc",
            "alg": "ES256"
        }
    ]
}
```
