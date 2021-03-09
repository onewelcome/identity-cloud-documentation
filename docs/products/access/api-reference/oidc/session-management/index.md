# Session Management

These APIs deal with [OpenID Connect Session Management 1.0](https://openid.net/specs/openid-connect-session-1_0.html). Onegini's OpenID Connect Provider (OP) 
offers the Check Session endpoint and a Relying Party Initiated logout endpoint as part of Session Management.

The urls for `check_session_iframe` and `end_session_endpoint` can be found in the [Discovery API](../discovery.md).

The two APIs are outlined here:

* [Check Session API](check-session.md)
* [End Session API](end-session.md)

For more information on the implementation and configuration, refer to the [topic guide](../../../topics/oidc/session-management/index.md).