# OpenID Provider iFrame

This is the [OpenID Provider (OP) iFrame](https://openid.net/specs/openid-connect-session-1_0.html#OPiframe). This will be used in coordination with the [Relying 
Party (RP) iFrame](../../../topics/oidc/session-management/relying-party-iframe.md) to keep track of the session state with the OP.

Endpoint: `GET /oauth/v1/checksession`

**Example OP iFrame html response**

```html
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>Check session</title>
<!-- necessary crypto libs included -->
<script>
  window.addEventListener('message', receiveMessage, false);

  function receiveMessage(e) {
    if (document.referrer.lastIndexOf(e.origin, 0) !== 0) {
      return;
    }
    if (typeof e.data !== 'string') {
      postStatus(e, 'error');
      return;
    }
    var messageTokens = e.data.split(' ');
    var clientId = messageTokens[0];
    var sessionState = messageTokens[1];
    if (typeof sessionState === 'undefined') {
      postStatus(e, 'error');
      return;
    }
    var salt = sessionState.split('.')[1];
    if (typeof salt === 'undefined') {
      postStatus(e, 'error');
      return;
    }
    var calculatedSessionState = calculateSessionState(clientId, e.origin, salt);
    var status = (sessionState === calculatedSessionState) ? 'unchanged' : 'changed';
    postStatus(e, status);
  }

  function postStatus(e, stat) {
    e.source.postMessage(stat, e.origin);
  }

  function calculateSessionState(clientId, origin, salt) {
    var opBrowserState = getOpBrowserState();
    return CryptoJS.SHA256(clientId + origin + opBrowserState + salt) + '.' + salt;
  }

  function getOpBrowserState() {
    var cookieName = 'opbs'; // The default cookie has a SameSite=None flag.
    var fallbackCookieName = 'LEGACY_opbs'; // The legacy cookie has no SameSite flag. iOS12 treats SameSite=None incorrectly.
    var cookie = getCookie(cookieName);
    if (cookie === '' && cookieName !== fallbackCookieName) {
      cookie = getCookie(fallbackCookieName);
    }
    if (cookie === '') {
      return '';
    }
    var sid = CryptoJS.enc.Base64.parse(cookie);
    return CryptoJS.enc.Utf8.stringify(sid);
  }

  function getCookie(name) {
    var nameWithSeparator = name + '=';
    var decodedCookie = decodeURIComponent(document.cookie);
    var cookies = decodedCookie.split(';');
    for (var i = 0; i < cookies.length; i++) {
      var cookie = cookies[i];
      while (cookie.charAt(0) === ' ') {
        cookie = cookie.substring(1);
      }
      if (cookie.indexOf(nameWithSeparator) === 0) {
        return cookie.substring(nameWithSeparator.length);
      }
    }
    return '';
  }
  /*]]>*/
</script>
</head>
<body></body>
</html>
```