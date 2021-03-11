# Mobile application version management

As organisation you only want approved application versions to be able to communicate with Onegini Access. Therefore application version management is an
important feature of Onegini Access. Each application version which is published must be configured. When talking about an application version a version of
the application for a specific platform is used. So for example: `PaymentApp v1.1 Android`. 

An application should prove it is the application version it pretends to be when it communicates with Onegini Access for the first time. When this is proven the 
application will receive credentials which can be used for further communication. Based on these credentials all requests for this application can be correlated
to this specific application installation.

In order to create a new application, go to the `Configuration` section of the administration console, then `App Configuration` and open the `Applications` tab. 
From the list of applications click on the application identifier in the `Identifier` column. At the bottom of the screen the versions for this application can 
be managed.

![App version list](img/app-version-list.png)

## Configure a new application version

![Add App version form](img/add-app-version-form.png)

Click on the `Add App version` button to create an App version from scratch. Click on the `Clone App version` button to create an App version with the same configuration as an existing version.

The `Version` fields need to be filled with the version identifier. The version identifier is a free format input field. Both numeric and textual input are accepted. It 
is advised to use numeric values to see the relation between versions in terms of which is the latest.

The status field has three options:

1. `Enabled`: this version of the application is allowed to register itself as a new application installation on Onegini Access and existing application installations can be used.
2. `Login enabled, registration disabled`: only applications that already had registered before can keep using Onegini Access without upgrading.
3. `Disabled`: completely disable the usage of a specific version. For example when a version configuration is created for future usage or the version contains some issue which requires it to be disabled. Or when a version is very old and customers are not allowed to use it anymore.

The application sends a signature to prove that is the version it pretends to be. This signature is set in the `Application signature` field. An app developer will
deliver this Application signature as described in the [App delivery lifecycle topic](../app-delivery-lifecycle/app-delivery-lifecycle.md#final-steps). When the application is in
development mode any value can be filled in as an Application signature because this value is ignored in development mode. When tampering detection is enabled the Application signature has to be hexadecimal 
formatted and be 32, 48 or 64 characters long. When an Application signature contains a `|`, this indicates that multiple architectures are supported. Every architecture requires a different Application signature.
The rules (when tampering protection is enabled) apply on all Application signatures separated by the `|` individually. So the following Application signature is not valid: 
`12345|ABCDEF1234567890ABCDEF1234567890` because the first part does not have the correct size.

>**Note:** For security reasons the secret will never be shown after it has been saved. You must enter a (new) secret when you clone an existing App version.

To prevent tampering with the application, for example via byte code manipulation, the `Tampering protection` checkbox should be checked. With tampering 
protection the application has to prove its identity to Onegini Access frequently.

For the communication of sensitive information TLS/SSL might not be sufficient. The `Payload encryption` feature adds an additional layer of encryption. To use
payload encryption a Onegini Security Proxy installation is required. Enabling `Payload encryption` without Onegini Security Proxy does not have any effect.

The `Integrity level` indicates if full application integrity check is performed. The value `Full` is using secure element of the device and is recommended way of checking app integrity. 
To prevent sudden upgrades of the applications after binary changes default behaviour can be changed  to `None`. By doing so, we change the way of application integrity calculation on the device.   

### Configure mobile authentication

>**Note:** The mobile authentication configuration is only available if the mobile authentication feature is enabled on system level.

#### Configure push messaging

`Push messaging configuration` can be selected from the dropdown list. It only shows the Push messaging configurations for the selected platform. For more 
information about it go to the [Mobile authentication page](../mobile-authentication/mobile-authentication.md).

#### Push messaging to Cordova app

This option is only visible for Android. Enable this when you send push messages to an Android app that is using Cordova. When enabled, extra
parameters are added to the push message. These are needed to receive the notification when the Cordova app is running in the background.

#### Use APNs development environment

This option is only visible for iOS. This specifies whether the production or development APNs environment is desired. By default, the production environment will be used.

#### Send badge number

This option is only visible for iOS. The badge is the number of unread notifications for an app shown on top of the app icon. When this setting is enabled, it will update the badge with the number of pending push authentications when the push message is sent.

>**Note:** This setting is only present for iOS because an iOS application does not get started until the user clicks on the notification while the android application is started in the background as soon as the notification is received by the device.

![iOS app icon with badge](img/ios-badge.png)

#### Set Application bundle identifier

This option is only visible for iOS. The Application bundle identifier is a unique identifier of an app. It uses the reverse domain name notation. You can find it in the [Apple developer account](https://developer.apple.com/account/ios/identifier/bundle).

## Remove an existing app version

Via the application version list a trash icon can be found. An app version can only be removed when no application installations are using the application 
version. An alternative to removing a version is [forcing end-users to upgrade](#forcing-end-users-to-upgrade).

## Forcing end-users to upgrade

There are several reasons why you would like end-users to upgrade their applications. For example:
* Reduce support costs due to less application versions.
* Act on known security vulnerabilities.
* Backwards incompatibility with backend systems.
* Force users to start using the latest features.

In order to force users to upgrade to a newer application version the old application version must be disabled. This can be achieved by editing the old version and 
set the Status to `Disabled`. Users will be prompted to upgrade via the app store when using the disabled application. After upgrading from an 
application version which is disabled the user can use the application again without having to register again.

>**Note:** Users might experience bad usability when they are forced to upgrade when users only have a mobile connection available. Forcing a user to upgrade 
makes the current application version unusable.

## Prevent new installation usage of the application

To prevent new application installations of an outdated version the version can be disabled for new registrations. This can be achieved by editing a version 
and set the Status to `Login enabled, registration disabled`. Users will be prompted to upgrade via the app store when using the application for the first time. 
Users that already used the application before won't notice any disturbance. After upgrading or downgrading from an application version which is disabled, the user
can use the application again without having to register again.

