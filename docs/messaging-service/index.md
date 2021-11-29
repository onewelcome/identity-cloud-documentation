# Messaging Service

## What is the Messaging service?

The *Messaging service* is functionality within the *OneWelcome Identity Cloud* that enables you to configure and send notifications to your customers via SMS
or email. The Messaging service serves as a notification gateway for other OneWelcome products to unify the configuration of the notification channels.

The Messaging service can send out SMS and emails with the data directly embedded in the request, or can use templates from [*Customer
Self-Styling*](../self-styling/index.md). Using templates gives you the option to adjust the look of the notifications.

Expected structure in Customer Self-Styling is:

```
tenant-specific-folder:
    | 
   messaging-service:
        |
        email:
        |   |
        |   html:
        |       - template.mustache
        |       - template2.mustache
        |   text:
        |       - template.mustache
        |       - template2.mustache
        sms:
        |   - template.mustache
        messages:
            - messages.properties
            - messages_en.properties
            - messages_pl.properties
            - messages_otherlocalecode.properties              
```

The Messaging service uses the [Mustache template language](https://github.com/samskivert/jmustache) for templating. Templates can be localized and sent in a
language specified in the request. All messages files MUST contain all translation keys. The Messaging service does not merge the default file with localized
files to avoid unnecessary file downloads. The locale must be passed in the request. If no locale is provided, the default file will be used. If no file for the
provided locale is present, the default file will be used. To include a localized message in the template, the message key should be wrapped in the `{{i18n}}`
tag. 

### Example

Template file:

```mustache
One, two, {{key}}. {{#i18n}}message.key{{/i18n}}!
```

Template parameters:

```properties
key=three
```

Messages.properties content:

```properties
message.key=Three elements Sir
```

When such a template is processed, `{{key}}` will be replaced by the value provided in the template parameters from the request,
and `{{#i18n}}message.key{{/i18n}}` will be replaced by the value from the properties file. The output will be:

```text
One, two, three. Three elements Sir!
```
