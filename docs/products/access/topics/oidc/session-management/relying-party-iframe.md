# Relying Party iFrame

This is only a sample of a Relying Party iFrame that could be used. Refer to the pseudo code for 
[Relying Party iFrame](https://openid.net/specs/openid-connect-session-1_0.html#RPiframe) on the OIDC specification site.

Comments have been added on specific lines to highlight actions items for implementation/configuration.

```html
<html xmlns="http://www.w3.org/1999/xhtml">
<head><title>OpenID Connect RP Session frame</title></head>
<body>

<script>
  /*<![CDATA[*/
  var targetOP = "https://OPserver.example.com"; //url of the OP
  window.addEventListener("message", receiveMessage, false);
  
  var scheduledCheck;

  window.onload = function () {
    // Check shortly after onload
    setTimeout(checkStatus, 2000);
    // Do not forget to deregister scheduled callback using clearInterval(checkStatus)
    scheduledCheck = setInterval(checkStatus, 1000*20); //Every 20 seconds
  };

  function getCookieValue(cookieName) {
    var name = cookieName + "=";
    var cookies = document.cookie.split(";");
    if (!cookies) {
      return null;
    }
    for (var i = 0; i < cookies.length; i++) {
      var cookie = cookies[i].trim();
      if (cookie.indexOf(name) === 0) {
        return cookie.substring(name.length, cookie.length);
      }
    }
    return null;
  }

  function checkStatus() {
    var client =  "oidc"; //this should be the clientId that is configured on the OP
    //You can name this cookie whatever you want, it should have been stored in a cookie after the Authentication response so it is accessible here
    var sessionState = getCookieValue("session_state"); 
    var text = client + " " + sessionState;
    var iframe = window.parent.document.getElementById("opif"); //this should be the id of the OP iframe you set when including them on the main page
    iframe.contentWindow.postMessage(text, targetOP);
  }

  function receiveMessage(event) {
    if (event.origin !== targetOP) {
      // Origin did not come from the OP; this message must be rejected
      return;
    }
    if (event.data === "unchanged") {
      // User is still logged into the OP, don't need to do anything
    } else {
      // Detected a change at OP
      // Perform authentication with `prompt=none` to check authentication status
    }
  }
  /*]]>*/
</script>
</body>
</html>
```
