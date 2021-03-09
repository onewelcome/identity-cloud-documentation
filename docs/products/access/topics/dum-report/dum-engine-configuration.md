# DUM engine configuration

The steps below summarizes how to configure the DUM (Delegated User Management) engine reports.

## Creating DUM engine configuration
Onegini Access allows to configure uri and credentials to access DUM engine instance.

In order to create a new configuration, go to the `Configuration` section of the administration console, then `System` and click the `Delegated User Management` tab.
On the overview that is shown you can see all the existing DUM engine configurations.

To add a new configuration click on the add button. The following form will appear:

![DUM Engine configuration](img/dum-engine-configuration.png)

| Field                             | Required  | Example value                                                           | Details
|-----------------------------------|-----------|-------------------------------------------------------------------------|------------------------------------------------------------------------
| Name                              | Yes       | myDumConfig                                                             | Descriptive name of the configuration
| DUM Engine uri                    | Yes       | https://dum.example.com                                                 | URI to access DUM engine instance
| Username                          | Yes       | myLogin                                                                 | Username to login to DUM engine instance
| Password                          | Yes       | gsD53F#da#$s35                                                          | Password to login to DUM engine instance

Fill all the mandatory fields (marked with `*` on the form). The other fields are optional.

## Configure mobile application to use selected DUM engine configuration
New `DUM integration` section will appear in mobile application configuration if at least one DUM engine configuration is present.

See the [Creating a new application](../mobile-apps/app-configuration/app-configuration.md#creating-a-new-application)
 for more information about this configuration.

![Mobile application DUM Engine configuration](img/app-dum-engine-configuration.png)

## Configure web client to use selected DUM engine configuration
New `DUM integration` section will appear in Web Client configuration if at least one DUM engine configuration is present.

See the [Creating a Web Client](../web-clients/web-client-configuration.md#creating-a-web-client) for more information about this configuration.

![Web client DUM Engine configuration](img/web-client-dum-engine-configuration.png)

