# End Session Endpoint

Endpoint: `GET /oauth/v1/logout?post_logout_redirect_uri=https://postlogout.example.com&id_token_hint=eyJraWQiOiJmNDYzYmYyYy04MWE2LTQ5Nzk`

## Parameters

Refer to the [RP Initiated Logout](https://openid.net/specs/openid-connect-rpinitiated-1_0.html) for more detail on the parameters. We have provided a 
brief description below:

| Parameter                     | Required     |                Description                                                                                      
|-------------------------------|------------- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------
| `id_token_hint`               | Recommended  | Previously issued ID Token passed to the logout endpoint as a hint about the End-User's current authenticated session with the Client                                                                          
| `post_logout_redirect_uri`    | Optional     | URL to which the RP is requesting that the End-User's User Agent be redirected after a logout has been performed
| `state`                       | Optional     | Opaque value used by the RP to maintain state between the logout request and the callback to the endpoint specified by the post_logout_redirect_uri query parameter

*Note*: that the `post_logout_redirect_uri` must have been previously configured on the client. It will only be respected if the `id_token_hint` is valid, it 
contains a valid client so the configuration can be retrieved, and that the URI matches the configured default or additional Post logout Redirect URIs. 
If no `post_logout_redirect_uri` is provided as a parameter, the default value in the client configuration will be used. If there is no configured URIs, it will
leave the user on a success page.

**Example response with valid post logout URI**

```html
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
  <title>Successfully logged out</title>
</head>
<body>
  <h1>Successfully logged out</h1>
  <script>
   window.onload = function () {
        window.location.href = 'yourpostLogoutRedirectUri';
   }
  </script>
</body>
</html>
```

## Front-Channel Logout support

Onegini's OP implementation also has support for [OpenID Connect Front-Channel Logout 1.0](https://openid.net/specs/openid-connect-frontchannel-1_0.html).

**Example response with front-channel with two RPs in the session**

```html
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
  <title>Successfully logged out</title>
</head>
<body>
  <h1>Successfully logged out</h1>
  <iframe style="display:block; visibility:hidden" src="https://rp1logouturl.example.com"></iframe>
  <iframe style="display:block; visibility:hidden" src="https://rp2logouturl.example.com"></iframe>
  <script>
   window.onload = function () {
        window.location.href = 'yourpostLogoutRedirectUri';
   }
  </script>
</body>
</html>
```
