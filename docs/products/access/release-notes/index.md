# Release notes

This section contains release notes for OneWelcome Access.

We are releasing OneWelcome Access on a 1-3 weekly basis. A release does not require downtime and will occur during European business hours. 

The releases are backward compatible. However, we will extend the API contract (adding new fields to a JSON object). If breaking changes are required in the API, a new API version is created, and the old version will be deprecated. Customers will have six months to migrate to the new API version. 

In the release notes, we mention new features and bug fixes. If anything is unclear, feel free to contact [OneWelcome Support](https://support.onewelcome.com).

## Release date 2023-12-19

### Features
* Added two new methods to the [User Sessions API](../api-reference/end-user/session.md) that make it possible to end all sessions for a given user or to end a specific session for a user.

### Improvements
* We improved our caching for the [Discovery](../api-reference/oidc/discovery.md) and JWKS endpoints so these endpoints can handle more load. 

## Release date 2023-12-11

### Features
* Implemented the new [Push Messaging configuration](../topics/mobile-apps/mobile-authentication/push-authentication.md#configuration-using-http-v1-fcm-apis--recommended-) for Android.
    * *Note*: All push messaging configuration for Android should be updated before June 20th, 2024. [Details about the migration](https://firebase.google.com/docs/cloud-messaging/migrate-v1)
* Certain high volume 'success' events have been excluded by default from being persisted to the database to improve performance.
    * These events can be configured via the Admin console. Please view our [topic guide](../topics/technical-app-management/events/events.md) for additional information.
* Added support for more signing algorithms for use with Private Key JWT (see `token_endpoint_auth_signing_alg_values` in the [Discovery API](../api-reference/oidc/discovery.md)).
* [Discovery API](../api-reference/oidc/discovery.md) now accepts `POST` requests.

### Improvement
* Invalid Grant [events](../appendix/access-events.md) now contain additional information for easier troubleshooting.

### Bug Fix
* Fixed an issue with [webhooks](../topics/integration-extension/hooks/index.md) when using `client_credentials` involving cache.

## Release date 2023-11-22

### Features
* We added an API to view all active sessions for a user. [User Sessions API](../api-reference/end-user/session.md)

### Improvements
* We adjusted the communication resiliency properties for Access to better handle issues with the external services. 

### Bugs
* The version of the backend for the mobile app config was fixed.
* Standard ID Token claims are now filtered from the custom registration user object.
* We ensured that the tenant context is always set when handling OneEx events.

## Release date 2023-11-03

### Features
* We enabled Custom Registration scripts to return user attributes in the ID Token and UserInfo Endpoint response.
* We have introduced a feature that allows for the removal of a user's Access Tokens and grants whenever they change their password. This enhancement ensures that a user's security and access control are maintained by revoking their existing access privileges and requiring them to reauthenticate with their new password.

## Release date 2023-10-30

### Features
* We now support more [`acr` values](../topics/oidc/overview/scopes-and-claims.md#acr). These `acr_values` are also exposed in the [Discovery Endpoint](../api-reference/oidc/discovery.md)
* We added [configuration](../topics/technical-app-management/session-config/session-config.md) to configure the SSO Session on a tenant level. This allows customers to set the default length and the maximum length of an SSO session.
* The user details cache (used for the ID Token and the userinfo endpoint) can now be cleared automatically if the connected IdP sends Events.

### Improvements
* Extended the token revocation events with a `User ID`.
* Improved error logging in [web hooks](../topics/integration-extension/hooks/index.md)

### Bugs
* Ensure the `acr` value is not lost when a specific attribute mapping is configured for an IdP.
* Removed some details from specific events.

## Release date 2023-10-09

### Features
* We added a new configuration item to Web clients. Specifying the Refresh Token validity from when the token is issued is now possible. We also clarified that the old config referred to the max lifetime, calculated from when the first access token was issued. Now, both validities can be configured. 
* If an account gets blocked or deleted from an IdP of the type CIM, we now clear all active tokens and remove all registered devices.

## Release date 2023-09-28

### Features
* The new `Map assertion attributes` feature for SAML IDPs, now also maps the attributes in the SAML assertion from the external IDP to claims in the UserInfo endpoint.

## Release date 2023-09-27

### Features
* We added a new `experimental feature` to connect to a new IDP type: ID Broker. This is a new component that supports federated authentication towards external identity schemes. 
* We added a new `Map assertion attributes` feature for SAML IDPs. This feature enables mapping all attributes in the SAML assertion from the external IDP to claims in the ID Token. Via Attribute mapping, you can limit this mapping to only include specific claims. If you have enabled the User Info integration, the assertion attributes will be overwritten if they have the same name.

### Improvements
* We improved the mapping of "expected error responses" from OAuth/OIDC IdP types, e.g. the `login_required` error is now mapped to this event `AUTHN REQUEST LOGIN REQUIRED` and no longer to `IDP OAUTH RECEIVED ERROR`. 
* We improved our production roll-out process. 
* We now also support a `.` character in the name of a scope.

### Bugs
* The length was limited to 20 characters when managing scopes via the API. We now aligned this to 255, which was already possible via the UI.
* We now support the new `urn:onewelcome:oauth2:grant_type:stateless_authentication` grant during the registration of a Mobile device. 

## Release date 2023-09-13

### Improvements
* Added `private_key_jwt` as `token_endpoint_auth_methods_supported` in the Discovery URL.
* Added a new event type, `TOKEN REQUEST EXPIRED REFRESH TOKEN`, which replaces the `TOKEN REQUEST INVALID REFRESH TOKEN` event when a refresh token is expired.
* Added a new event type, `TOKEN REQUEST FINGER PRINT EXPIRED REFRESH TOKEN`, which replaces the `TOKEN REQUEST FINGER PRINT INVALID REFRESH TOKEN` event when a refresh token is expired.
* We started logging the length of a refresh token in the details of the `TOKEN REQUEST INVALID REFRESH TOKEN` event.  

## Release date 2023-09-08

### Improvements
* We added a new attribute: `baseUrl` to the modelmap of the `endsession`-templates.

### Bugs
* Resolved an issue where the `auth_time` was not updated after re-authentication based on the `max_age` parameter.

## Release date 2023-09-05

### Bugs
* Resolved an issue where clearing caches via the Admin UI did not work.
* Resolved an issue where message keys were always cleared after 5 minutes. The TTL now follows the configured duration for static resources. 

## Release date 2023-08-31

### Features
* We added the optional `azp` claim to the [ID Token](https://openid.net/specs/openid-connect-core-1_0.html#IDToken).
* We introduced [stateless custom registration](../topics/custom-registration/stateless-custom-registration.md).

### Improvements
* The `Respond to silent authentication requests based on current session state` feature now redirects to the upstream IDP in case additional `scopes` are requested, a specific `acr_value` is requested, or when `max_age` indicates that the `auth_time` needs to be refreshed.

### Bugs
* Resolved an issue where a Logout request failed if the `id_token_hint` contained a token with multiple audiences. 

## Release date 2023-08-07

### Features
* We have optimized how we handle our Single sign-on (SSO) experience when multiple clients use the same IdP. We decided to use the existing OneWelcome session when new clients initiate an authentication request with `prompt=none` and the `Respond to silent authentication requests based on current session state` feature is enabled for that client. Before, the request was forwarded to the configured IdP for that web client. 

## Release date 2023-08-03

### Improvements
* We added an additional setting for the App2Web integration for the Tulip type idp that allows setting the "Used authentication methods". This is required for setups where 2FA is required on the Tulip side.

## Release date 2023-08-02

### Bugs
* We resolved another bug in the SLO request towards a Tulip type idp.

## Release date 2023-07-27

### Features
* It is now possible to [remove Identity Providers via API](../api-reference/config-api/identity-provider-api.md).

### Bugs
* We resolved a bug in the SLO request towards a Tulip type idp.
* We resolved a bug where missing templates uploaded via self-styling caused the app the fail.
* A `PATCH` call the the [Web Client API](../api-reference/config-api/web-client.md) resulted in the ` session_based_silent_auth` value to be overwritten with `false`.

## Release date 2023-07-14

### Improvements
* We simplified the App2Web integration between Access and Tulip.

## Release date 2023-07-11

### Features
* We extended the [ConfigAPI](../api-reference/config-api/identity-provider-api.md) with the option to create IDPs of the type `Tulip` and `OAuth`.

### Improvements
* We introduced caching for Tulip's JWKS endpoint, so the certificates are no longer fetched during every authentication.

### Bugs
* Fixed a bug that caused calls to the [ConfigAPI](../api-reference/config-api/identity-provider-api.md) to fail when the Authenication Method was set to PrivateKeyJWT. 

## Release date: 2023-06-30

### Bugs
* We fixed the support for key rotation for clients authenticating with PrivateKeyJWT.  

## Release date: 2023-06-29

### Improvements
* We have expanded the details of the log events published by Access when handling OIDC/OAuth callbacks fails.  

## Release date 2023-06-22

### Features
* We now support forwarding a logout request, initiated in Onewelcome Access, towards an IDP of the type Tulip. 

### Bugs
* We fixed an issue where the admin panel showed different values for a configured client for a limited time after an update of the configuration
* We added the `sub` claim to the Token Introspection response for Client Credentials grants to become compliant with [RFC7523](https://datatracker.ietf.org/doc/html/rfc7523#section-3).

## Release date 2023-06-05

### Improvements
* We now filter disabled external IDPs when the mobile SDK requests the client configuration.

## Release date 2023-06-02

### Improvements
* We now allow a scope name to be 255 characters.
* We introduced a new configuration to set the minimum time frame for allowing multiple `Access Tokens` for a given user and client to coexist.

## Release date 2023-05-30

### Features
* Added support for privateKeyJWT client authentication for the `Tulip` type IDP.
* Added support for the App2Web feature for the `Tulip` type IDP.

## Release date 2023-05-18

### Improvements
* We now also support colons `:` in the name of scopes.

## Release date 2023-05-10

### Improvements
* We now allow `HEAD` requests to the authorization URL.

### Bugs
* Fixed a bug where some cache values did not expire on time.

## Release date 2023-05-05

### Bugs
* Fixed an issue where a request to a SAML IDP timed-out before the web session expired. This can happen when users have to go through an registration process at the IDP.

## Release date 2023-05-03

### Features
* Performance improvements
* Added experimental support for a new way to connect to the `Tulip` type of IDP.

### Improvements
* Removed support for `RSA1_5` and `RSA_OAEP` as encryption algrithms.

## Release date 2023-04-25

### Bugs
* Fixed a bug that prevented updating the SAML SP configuration.

## Release date 2023-04-18

### Bugs
* Fixed a bug where the SAML SP configuration was sometimes resolved incorrectly.

## Release date 2023-03-31

### Improvement
* We now support the `client_id` parameter combined with the `post-logout-url` for the OIDC logout endpoint.
* We improved the error handling for all `OAuth` and `Tulip` type IDPs.

### Bugs
* Not all claims in the ID token were forwarded for Identity Providers of the type `Tulip`. This has been fixed.
* We solved an issue where a `null` value in the Hook response caused an error. 

## Release date 2023-03-08

### Features
* Added support for `client_secret_post` authentication on calls to the `/token` endpoint for IDPs of the type Tulip or OAuth.

### Improvement
* [Mapped user attributes](../topics/general-app-config/identity-providers/identity-providers.md#attribute-mapping) are now also accessible in the [Access Token Webhook](../topics/integration-extension/hooks/customize-access-token/customize-access-token-hook.md).
* Improved the caching of the default templates and messages.

### Bugs
* When a Web client requests scopes in an authentication request which are not known in the connected IDP of the type Tulip, we now forward the error to the requesting client.

## Release date 2023-02-27

### Improvement
* Added support for setting [AMR](https://www.rfc-editor.org/rfc/rfc8176.html) when using [custom registration](../topics/custom-registration/index.md)
    * Requires the [Extension Engine](https://docs-single-tenant.onegini.com/msp/stable/extension-engine/release-notes/2.x.html) `2.5.0` and above.

## Release date: 2023-02-21

### Improvement
* All available user-related claims are now accessible in the [Access Token Webhook](../topics/integration-extension/hooks/customize-access-token/customize-access-token-hook.md)

### Bugs
* Cloning a [mobile app](../topics/mobile-apps/index.md) via the [Application version API](../api-reference/config-api/applications/application-version-api.md) works again
* The [User Details Customization hook](../topics/integration-extension/hooks/customize-user-details/customize-user-details-hook.md) is no longer cleared when editing a [web client configuration](../topics/web-clients/index.md) in the Admin console UI

## Release date: 2023-02-16

### Bugs
* Fixed an issue where a user was not always logged out when they had two sessions, one based on [Cookie based authentication](https://docs-single-tenant.onegini.com/cim/stable/idp/authentication/saml/cookie-based-authentication.html), and a 'regular' authentication.

## Release date: 2023-02-08

### Bugs
* Fixed an issue where requests towards a configured SAML IDP (e.g. `CIM`) failed when a single user-agent initiated multiple authentication requests for the same client.

## Release date: 2023-02-06

### Features
* Added support for [custom parameters](../api-reference/description-oauth-endpoint.md#hook_context_custom_param) in the [Customize User Details Web Hook](../topics/integration-extension/hooks/customize-user-details/customize-user-details-hook.md).
* [Custom Registration](../topics/custom-registration/index.md) scripts now can also access [custom parameters](../api-reference/description-oauth-endpoint.md#hook_context_custom_param), this makes it possible for the script to execute different logic based on the provided params.

### Improvements
* For the IDP type `Tulip`, we now send all the requested scopes (default, optional, and the scopes in the IDP config) to the IDP in the authentication request.
* We improved the performance of Silent authentication requests (`prompt=none`) towards an IDP of the type `Tulip`.

## Release date: 2023-01-24

### Features
* The end-user is now redirected to a whitelisted `post-logout-url` after a OIDC logout request, even when the provided ID token is recently expired or there is no active session for that client.

### Bugs 
* The [Device API](../api-reference/end-user/device-v4.md) now returns a timestamp for `lastLogin`.
* The `removeScopes` feature in the [Customize Access Token Web Hook](../topics/integration-extension/hooks/customize-access-token/customize-access-token-hook.md) will now be respected when used together with [Custom registration](../topics/custom-registration/index.md).

## Release date: 2023-01-18

### Improvements
* For [Custom registration events](../appendix/access-events.md#custom-registration-audit-events), we now log the `Client id` to make it easier to correlate the events to a client or mobile device.

### Bugs
* While adding a mobile authentication type "SMS", the `SMS sender id` always returned a validation error in the UI. This is now fixed.

## Release date: 2023-01-11

### Features
* We introduced a new `Tulip` Identity Provider Type.

## Release date: 2022-12-15

### Bug fixes
* Fixed a bug where the introspection endpoint returned a `rpSet` claim instead of user attributes.
* We now also remove the Mobile push message capabilities for users that remove their last mobile device. 

## Release date: 2022-12-01

### Features
* Next to an instance in the EU, we also released an instance in the US.

## Release date: 2022-11-30

### Improvements
* We improved the performance and stability of the application.

## Release date: 2022-11-11

### Improvements
* We improved the performance caused by clients with multiple redirectUris.
* We improved the performance of the Events page for customers with mobile devices.

## Release date: 2022-11-01

### Bug fixes
* Fixed a bug where Access tokens were not revoked after a logout request if the session was created based on a cookie.

## Release date: 2022-10-11

### Features
* Added an API to delete all tokens for a specific user per type.

### Bug fixes
* Fixed an issue where token introspection showed claims with the value `null`. These are now hidden. 

## Release date: 2022-08-29

### Features
* Support for custom registration for Web clients.

## Improvements
* Reduced the number of calls to our caching database for templates.

## Release date: 2022-08-16

## Improvements
* We introduced a `v2` of the token introspection endpoint to comply with RFP7662 for the `exp` attribute.
* We now make a SAML SLO request succeed even without a session, based on SpNameQualifier in the SAML metadata.

## Release date: 2022-07-06

### Improvement
* Removed support for the deprecated algorithms `RSA1_5` and `RSA_OAEP`.
* The generated `Server Public Key` is now visible as text on the mobile app's configuration page.

### Improvement
* Fixed an issue that caused exceptions when making calls to our caching database.

## Release date: 2022-06-29

### Features
* It is now possible to add an [extra param `hook_context_custom_param.*`](../api-reference/description-oauth-endpoint.md) to the authorization endpoint. This param is then available in the [Onegini Customize Access Token Web Hook](../topics/integration-extension/hooks/customize-access-token/customize-access-token-hook.md) as context.

## Release date: 2022-06-09

### Features
* In the authentication response, we now indicate which external IDP was used by the end-user to authenticate. E.g. when a user uses Digid, we fill the `acr` with `urn:com:onegini:saml:idp-alias:digid`.

### Improvement
* It's now impossible to configure an API based custom authenticator with the PCKE grant type.

### Bug fixes
* We aligned the `exp` value in our token introspection endpoint with the RFC 7662. It now is an integer timestamp, measured in the number of seconds
since January 1 1970 UTC. To use this new value, please switch to the v2 of our token introspection endpoint.

## Release date: 2021-09-14

### Features

* Added the ability to delete access and refresh tokens when using the [End-session endpoint](../topics/oidc/session-management/end-session-guide.md) for OpenID
  Connect. This is enabled by default for clients with the authentication method `PKCE`. Refer to the  
  [OpenID Connect configuration](../topics/oidc/configuration/configuration.md) for more information
* Added the [token revocation endpoint](../api-reference/description-oauth-endpoint.md#revoke-token-endpoint) to
  the [OpenID Connect discovery endpoint](../api-reference/oidc/discovery.md).
* Improved the [integrity check](../topics/mobile-apps/app-delivery-lifecycle/app-delivery-lifecycle.md) for mobile apps. This improved integrity check is
  required for new mobile apps introduced to the Google Play Store after August 1st, 2021. The existing apps, both running on Android and iOS, will continue to
  work without any changes. Still, it is recommended to plan an update of the Onegini SDK and use the improved integrity check.

### Bug fixes

* The OpenID End-session endpoint did not properly handle encoded parameters when it was called via HTTP POST. This prevented users from being redirected back
  to the website after logout. This has been fixed.
* Several endpoints for Mobile Authentication returned responses in a different format than documented. These responses have been fixed.

## Release date: 2021-05-10

### Features

* It is now possible to [configure multiple redirect URLs](../topics/mobile-apps/app-configuration/app-configuration.md) for mobile apps. This makes it possible
  to change the app scheme of the mobile app in a new version while the existing app installations use the old app scheme.
* Relying Parties (RP) can resolve user attributes from Access by calling the User Info endpoint or by requesting an ID Token. Both means are defined by OpenID
  Connect (OIDC) standard. The returned set of identity related claims couldn't be modified, extended or filtered other than by using scopes. With the new User
  Details Customization Web Hook serves these purposes.
* When the user device domain state changes, Access will publish a corresponding event to the event bus notifying all interested parties about the change.
  Device domain state changes are: a new user device registration, deregistration, user logging in with the device, or mobile authentication enrollment changes.
* The process of registering a new mobile application requires both parties, the device, and the server, to have their time/date settings set correctly. Some
  users are explicitly modifying their time which prevents them from successfully finishing the onboarding process. To improve the user experience Access will
  handle such situations more gracefully by detecting clock skew and informing the client about the root cause of the rejection.

### Improvement

* When Access failed to successfully send a PUSH notification via Apple Push Notification Service (APNS), it returned a generic error to the client. To help
  diagnose the root cause of the issue, Access will log more detailed information about why the notification got rejected in the corresponding event.

### Bug fixes

* The mobile applications that were using the [Custom Registration](../topics/custom-registration/index.md) feature had to send additional request in order to
  obtain an ID Token. This will no longer be required as the ID Token will be returned to the client along with the Access Token when configured.
* Users who are either members of many Delegated Administration for Business Partners ([DABP](../../../dabp/index.md)) groups or are having many DABP policies
  assigned, could experience issues when logging out from DABP or Onegini Console applications. The logout request in such scenarios will no longer be rejected.

## Release date: 2021-03-09

* First official release of the access components.
