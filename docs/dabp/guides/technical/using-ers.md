# Using Customer Self Styling for customizing looks and behaviour of DABP

Look and some behaviour can be customized by using [Customer Self Styling](../../../self-styling/index.md)
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

By default when one user invites another to use DABP, details of the inviting users are provided in the invitation email. Since DABP allows you to invite users
in the automated way by API, there are no inviting user details for the email. In that case a `system automation` account data will be put in the email. Email
address and display name of this account can be changed using the translation file. Please provide such entry in the translation file to change default values:

```json
{
  //..other translation keys
  "system-automation": {
    "email": "system-automation@onewelcome.com",
    "display-name": "System automation"
  }
}
```

