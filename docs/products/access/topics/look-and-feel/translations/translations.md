# Translations

Onegini Access supports internationalization of templates via messages. For each message multiple translations can be set. This topic guide will explain how
to configure the translations of messages used in templates. These translations follow the <abbr title="Internationalization">i18n</abbr> standard for messages. 

## Configure the supported language(s)

A translation can be configured for the locales supported by Onegini Access. The supported locales can be configured via the admin console:
System &rightarrow; General &rightarrow; Supported Locales.

## Adding or updating a translation

When no translation is found for a message key, Onegini Access will look for an available default. These defaults are available for the standard Onegini 
templates. When using custom templates we advise to pick different message keys per template set, so for these messages no defaults will be available without
any configuration.
  
Configuration of a message key with corresponding translations is done by specifying the message key plus at least a default content. The default content is 
used if no matching translation is found for the user's language. For all supported locales a separate content field is available to set the translation. If the
content is left empty for a specific language it is treated as unset and the default will be used. If for example English is your default language but you 
also support Dutch you only need to set the default and Dutch content and not the English content. In this example the default content is used for English.

If only a single locale is supported this locale is seen as the default so only the default content field is available. 

For parametrized messages use the `{n}` notation. To use for example the first provided parameter in your message: `You have {0} attempts left`. 

>*Note*: The translation are cached in Onegini Access engine to limit database queries. Therefore it can take some time before a change in a translation
has effect. The time to live for this cache can be configured via property: [`Message cache TTL`](../../technical-app-management/cache-config/cache-config.md).

## Adding translations

Translations can be imported to the application by using the message files within [Customer Self Styling Application](https://onegini-onegini-external-resources-services.readthedocs-hosted.com/en/latest/configuration-page/).
Add property file to the template set in your github repository to make use of the customized templates.

```
 - messages.properties
 - messages_locale.properties
```

For example:
```
 - messages.properties
 - messages_nl.properties
 - messages_de.properties
```

Imported messages will overwrite existing translations. Only the message files for the supported locales will be imported. These locales can be configured via 
the admin console: System &rightarrow; General &rightarrow; Supported Locales. The file `messages.properties` must be 
present. This file contains the default translations that are used when a specific translation is missing for a language.


### Translation files

The translation files must follow the Java [.properties file format](https://en.wikipedia.org/wiki/.properties). This means that for each translation there is 
a key and a value separated by an equals sign: `button.submit=Submit`. Each translation must be placed on a new line. The length of each translation value is 
limited to 1024 characters.

When the translation contains characters with diacritics (é, ü, ž) or special characters that are not present on a US keyboard, the safest option is to 
[convert them](https://itpro.cz/juniconv/). Example: convert `button.verify=Verifiëren` into `button.verify=Verifi\u00EBren`