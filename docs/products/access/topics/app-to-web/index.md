# App To Web Single Sign On

App to Web Single Sign on allows you to take a session from your mobile application and extend it to a browser on the same device. This is
useful for giving a seamless experience to your users when they transition from the mobile application to the website where more
functionality likely exists. 
This feature is available when using the Tulip identity provider, 
the [OneWelcome CIM identity provider](../general-app-config/identity-providers/identity-providers.md#configure-a-onewelcome-cim-identity-provider),
or the [OneWelcome CIM API identity provider](../general-app-config/identity-providers/identity-providers.md#configure-a-onewelcome-cim-api-identity-provider).
In CIM, OneWelcome Access uses the ActionToken Type `LOGIN_APP2WEB`, which has been available since OneWelcome CIM version 6.2.0.
In Tulip, OneWelcome Access uses a Authentication Token, this can be used with Tulip R103 and up.

The OneWelcome SDK allow you to specify a target URL where authentication is required. This URL must be configured in the
[Action Token](https://docs-single-tenant.onewelcome.com/cim/stable/idp/topic-guides/authentication/action-token-login.html) configuration. It
will then verify that your mobile
application's session is valid and establish a session with the IDP before redirecting the user to the target URL with them logged in
automatically.
Please refer to the OneWelcome SDK documentation for the specifics on the implementation:

* [Android](https://developer.onewelcome.com/android/android-sdk/app-to-web-single-sign-on)
* [iOS](https://developer.onewelcome.com/ios/sdk/app-to-web-single-sign-on)
