# User Info API

This API is the implementation of UserInfo endpoint defined in [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html#UserInfo) specification.
It returns claims for the user associated with the AccessToken that is presented via Bearer Authentication. 

This endpoint requires Bearer Authentication. The token must be a valid AccessToken with at least `openid` scope.
A `200 OK` is returned when the user presents a valid token. Otherwise, one of the following errors is returned:

| Http status code | Description                                                                      |
|------------------|----------------------------------------------------------------------------------|
| 401              | No bearer token present in the request, the token does not exist or has expired  |
| 403              | The token does not have `openid` scope                                           |

Endpoints: 

* `GET /oauth/v1/userinfo`
* `POST /oauth/v1/userinfo`

This endpoint accepts the following request parameter(s) 

| Parameter            | Required | Example value | Description
|----------------------|----------|---------------|---------------------------------------------------------------------------------------------------------
| refresh_user_details | no       |  true         | The values of the claims from the [user info endpoint](../../topics/general-app-config/identity-providers/identity-providers.md#configure-user-info-endpoint) are cached after the first request. Use this parameter to force fetching the user details again from the user info endpoint. Performance may decrease when you add this parameter to every request.  


**Example request**
```http
GET /oauth/v1/userinfo HTTP/1.1
Content-Type: application/json;charset=UTF-8
Authorization: Bearer 792D84C0509CFC589A3F25BC55FD4A72756B191B5682956B273F2A695EC20FE9
Host: example.com
```

**Example response**

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
 
{
    "sub": "2c32f622-ecfe-42a7-a25c-7a2c674acb4f",
    "name": "John Doe",
    "preferred_username": "John Doe",
    "given_name": "John",    
    "family_name": "Doe",
    "nickname": "John Doe",
    "birthdate": "1980-01-01",
    "email": "john.doe@example.com"
    "email_verified": false,
    "phone_number": "+48505500500",
    "phone_number_verified": false,
    "address": {
        "locality": "Woerden",
        "street_address": "Tjaskermolenlaan 1",
        "country": "Netherlands",
        "postal_code": "3447 GE",
        "region": "Utrecht"
    },
    "gender": "male",
    "locale": "nl"
}
```
