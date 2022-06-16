# Release notes

This section contains release notes for Onegini Access.

In the release notes we mention new features and bug fixes. If anything is unclear, feel free to contact [Onegini Support](https://support.onegini.com).

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
