# App To Web Single Sign On

App to Web Single Sign on allows you to take a session from your mobile application and extend it to a browser on the same device. This is useful for giving
a seamless experience to your users when they transition from the mobile application to the website where more functionality likely exists. This functionality
can only be used when using the [Onegini CIM identity provider](../general-app-config/identity-providers/identity-providers.md#configure-a-onegini-cim-identity-provider)
or the [Onegini CIM API identity provider](../general-app-config/identity-providers/identity-providers.md#configure-a-onegini-cim-api-identity-provider) 
as it is a unique feature of the [Onegini CIM](https://docs-single-tenant.onegini.com/cim/stable/idp). Onegini Access
uses the ActionToken Type `LOGIN_APP2WEB` which is available since Onegini CIM version 6.2.0.

The Onegini SDK allow you to specify a target URL where authentication is required. This URL must be configured in the 
[Action Token](https://docs-single-tenant.onegini.com/cim/stable/idp/topic-guides/authentication/action-token-login.html) configuration. It will then verify that your mobile 
application's session is valid and establish a session with the IDP before redirecting the user to the target URL with them logged in automatically. 
Please refer to the Onegini SDK documentation for the specifics on the implementation:

* [Android](https://docs-single-tenant.onegini.com/msp/stable/android-sdk/topics/single-sign-on.html)
* [iOS](https://docs-single-tenant.onegini.com/msp/stable/ios-sdk/topics/app-to-web-single-sign-on.html)
