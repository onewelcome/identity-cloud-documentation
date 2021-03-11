# General system properties configuration

In order to configure Onegini Access general properties, go to the `Configuration` section of the administration console, then `System` and choose the `General` tab.

The picture below shows the `General` view in the Admin Console with an example configuration.

![General Configuration](img/general-config-form.png)

The table below describes the general properties that can be configured.

>**Note:** Changes to the User authentication session TTL value will only affect new entries. The TTL values of existing entries will not be updated.

| Field name                      | Default | Description                                                                                                                                                                            
|---------------------------------|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| User authentication session TTL | 360     | To enable some authentication features like SAML an authentication transaction is used. The time to live in seconds for the user authentication session can be set via this parameter.
| Engine Base Urls                |         | These urls will be selectable when exporting the app configuration. They will also be used to show example URLs in a few different places in the admin console.