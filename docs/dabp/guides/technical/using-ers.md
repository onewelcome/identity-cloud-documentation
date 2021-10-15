# Using Customer Self Styling for customizing looks and behaviour of DABP

Look & feel and some behaviour can be customized by using [Customer Self Styling](../../../self-styling/index.md)
The base folder in the git repository for DABP should be named `delegation`.

## Changing look and feel

You can change the colors, theme and favicon of the application by providing proper files in git repository configured in the Self Styling application.
Supported files:

- palette.json - to customize theme of the application
- logo.svg - to customize logo of the application
- favicon.ico - to customize favicon of the application
- translation.json - to change the texts used in the application and emails

DABP supports multiple languages, thus translation files should be stored in git in `delegation/locales/<language_code>/translation.json`, for example
`delegation/locales/en/translation.json` to change english texts or `delegation/locales/nl/translation.json` to change dutch texts.

## Using translations to change invite email texts

By default, when one user invites another to use DABP, the details of the inviting users are provided in the invitation email. DABP also allows users 
to be invited in an automated fashion using the API. In this context, there is no user details of the inviter for the email. In this scenario, some 
pre-defined `system automation` data will be used in the email. The Email address and display name of this account can be changed using the translation
file. You'll need to provide an entry in the translation file to overwrite default values:

```json
{
  //..other translation keys
  "system-automation": {
    "email": "system-automation@onewelcome.com",
    "display-name": "System automation"
  }
}
```

