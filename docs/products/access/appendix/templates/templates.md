# Templates

This section covers the template customizations possible in the OneWelcome Access.

The OneWelcome Access has a number of templates that can be customized. OneWelcome uses the Thymeleaf template engine to render templates. It takes XML (XHTML) as
input and replaces static content using special attributes. Refer to the
[Thymeleaf manual](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html) for the full syntax.

> *Note*: The mentioned available messages are the default messages. Please refer to the [Translations topic guide](../../topics/look-and-feel/translations/translations.md) on how to add custom messages.

## Consent template

### Introduction

The consent page will be displayed to the end user when a client requests an access grant and the user did not provide consent to any of the requested scopes
for this particular client.

As the application is stateless this page has the responsibility to forward all request parameters used in the authorization request. This set of request
parameters is available via variables in the template. When posting the request, the names of the request parameters used should be according to the OAuth 2.0
specification for requesting an access grant. The endpoint to post the consent confirmation is `/oauth/consent`.

### Naming convention

The name of the template file has to be `consent.html`.

### Available messages

| Key                 | Description                                                                                                                                                             |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| pages.consent.title | Page title to be used in the browser title.                                                                                                                             |
| consent.header      | Page title to be used in a heading. The client name can be provided to be included in the message via a parameter.                                                      |
| consent.intro       | Describing text to inform the user what this page is about and which action is required. The client name can be provided to be included in the message via a parameter. |
| consent.scopes      | Title heading the list of requested scopes.                                                                                                                             |
| consent.submit      | Label used for the submit button.                                                                                                                                       |

### Available variables

| Variable name     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|-------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| clientName        | Name of the client that requests authorization                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| requestedScopes*  | Space delimited string of scopes that are requested.                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| clientId*         | Client id used in the access grant request.                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| responseType*     | Response type used in access grant request.                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| state*            | State used in the access grant request.                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| redirectUri*      | Redirect uri used in the access grant request.                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| nonce*            | Nonce used in the access grant request.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| idp               | Id of the Identity Provider that should be used to authenticate client                                                                                                                                                                                                                                                                                                                                                                                                                     |
| requestedScopeMap | Map of scopes requested. As a key the name/id of the scope is used. The value contains the description of the scope. The language of the description is based on the user’s language setting or browser locale when no user language is set. When no description is available for the user language, the English description is used. When no description is available in the user’s language or the English language, the scope is not available in this map.                             |
| csrfToken*        | The CSRF token is used to prevent Cross Site Request Forgery. The token is a 320 character HEX string. The token is user, client and scope specific so can only be used for the set of parameters specified in the request. To ensure no replay attacks can be performed with the token, the token is only valid for 5 minutes. When a token is expired or incorrect, the user is redirected back to the consent screen.                                                                   |
| clientLogoUri     | Uri of the logo used for the client requesting consent. If the client has no logo this variable will be null.                                                                                                                                                                                                                                                                                                                                                                              |
| user              | This variable contains the user object of the user that authenticated. Based on the identity provider configuration used the available information set in this object can differ. The object contains a map with all user attributes including all custom attributes. Values in the map should be treated as non trusted, therefore th:text or the #strings.escapeXml() function should be used when displaying a value. Example usage `<p th:text="${user.attributes['firstName']}"></p>` |

* MUST be included in the request that posts the consent.

## Forward to authentication

The forward to authentication page can be shown before the user is sent to a SAML or OAuth identity provider. The default template redirects the end user to an
endpoint that initiates login at the identity provider.

### Naming convention

The name of the template must be `forward-to-authentication.html`.

### Available messages

| Key                                 | Description                                                                                                 |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------|
| pages.forwardToAuthentication.title | Title shown in the browser tab.                                                                             |
| forwardToAuthentication.title       | Title shown on the page.                                                                                    |
| forwardToAuthentication.body        | The paragraph informing that the end-user needs to sign in. Should redirect the end-user to the next step.  |

### Available variables

| Variable name     | Description                                                                                                        |
|-------------------|--------------------------------------------------------------------------------------------------------------------|
| forwardTo         | The page the end user is sent to                                                                                   | 

## Authorization complete

The authorization complete page will be displayed when the access grant is generated and the user is redirected back from the browser to the mobile application.
This feature should only be used in combination with a native browser. The main responsibility of the page is performing a JavaScript redirect and displaying a
message to the end user that the enrollment flow is completed.

### Naming convention

The name of the template file has to be `authorization-complete.html`.

### Available messages

| Key                                | Description                                                                                                      |
|------------------------------------|------------------------------------------------------------------------------------------------------------------|
| pages.authorization_complete.title | Page title to be used in the browser title.                                                                      |
| authentication.complete            | Message to the end user that the flow is successfully completed and the button should be pressed to continue.    |
| authentication.complete.error      | Message to the end user that the flow is completed with an error and the button should be pressed to continue.   |
| authentication.complete.continue   | Label for the continue button.                                                                                   |

### Available variables

| Variable name     | Description                                                                                                                            |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| redirectUri       | Uri the user should be redirected to.                                                                                                  |
| clientLogoUri     | Uri of the logo used for the client requesting authorization. If the client has no logo this variable will be null.                    |
| error             | An error that occurred during the authorization flow, example values: `unsupported_response_type`, `invalid_request` , `invalid_scope`.|
| errorDescription  | Description of the error that occurred during the authorization flow.                                                                  |
| errorUri          | Occasionally, the error may also contain an URI pointing to the error page.                                                            |

## Error Template

#### Naming convention

The name of the template file has to be `error.html`.

### Variables

| Variable name        | Description                                                                                                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| exceptionTitle       | The title of the exception. The exceptionTitle is resolved as a message key.                                                                                                            |
| exceptionDescription | Description of the exception. The exceptionDescription is resolved as a message key.                                                                                                    |
| exceptionErrorCode   | An error code value that can be mapped to some specific error description. Useful when redirecting to mobile apps. This may be populated when the there is an error with a SAML request.|

### Available messages

| Key                                   | Description                                   | Note                                                                                              |
|---------------------------------------|-----------------------------------------------|---------------------------------------------------------------------------------------------------|
| error.generic                         | The generic error message.                    |                                                                                                   |
| error.invalid_request.title           | Title of the invalid request error.           | If an invalid request is detected this key is the value of the exceptionTitle variable.           |
| error.invalid_request.description     | Description of the invalid request error.     | If an invalid request is detected this key is the value of the exceptionDescription variable.     |
| error.unauthorized_client.title       | Title of the unauthorized client error.       | If an unauthorized client is detected this key is the value of the exceptionTitle variable.       |
| error.unauthorized_client.description | Description of the unauthorized client error. | If an unauthorized client is detected this key is the value of the exceptionDescription variable. |

## OpenID Connect

### Check session status

This template is used within an iframe of an OpenID Relying Party (RP) to check whether the user has a valid session. The iframe is usually not visible to the
end user. The default template contains only JavaScript.

#### Naming convention

The name of the template file must be `check-session.html`.

### Variables

| Variable name    | Description                                                                                               |
|------------------|-----------------------------------------------------------------------------------------------------------|
| cookieName       | Name of the cookie that contains the OpenID Provider browser state. This cookie has a SameSite=None flag. |
| legacyCookieName | Name of the cookie that contains the OpenID Provider browser state. This cookie has no SameSite flag.     |

#### Available messages

| Key                             | Description                                 |
|---------------------------------|---------------------------------------------|
| pages.openid.checksession.title | Page title to be used in the browser title. |

### End session confirm

As a security measure the user can be asked to confirm to end their session. To accept the logout, post the form to `/v1/logout/accept`. To reject the logout,
add a link to `/v1/logout/reject`.

#### Naming convention

The name of the template file must be `endsession-confirm.html`.

#### Available messages

| Key                                   | Description                                     | Note                        |
|---------------------------------------|-------------------------------------------------|-----------------------------|
| pages.openid.endsession.confirm.title | Page title to be used in the browser title.     |                             |
| openid.endsession.confirm.header      | Title of the page in the body.                  |                             |
| openid.endsession.confirm.body        | Paragraph to ask the user to confirm the logout |                             |
| openid.endsession.confirm.accept      | Submit button to accept the logout.             |                             |
| openid.endsession.confirm.reject      | Link to reject the logout.                      | Link to `/v1/logout/reject` |

#### Variables

| Variable name          | Description                                                                                                                                               |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| T(com.innovation_district.common.util.CsrfUtil).CSRF_TOKEN | The CSRF token is used to prevent Cross Site Request Forgery when accepting the logout. When a token is expired or incorrect, the user is redirected back to this confirm screen. |

### End session success

This page is shown when the OpenID session has been invalidated.

#### Naming convention

The name of the template file must be `endsession-success.html`.

#### Variables

| Variable name          | Description                                                                                                                                               |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| frontChannelLogoutUris | A list of URIs to end the session of the user at these Relying Parties. This list can be empty.                                                           |
| postLogoutRedirectUri  | The user should be redirected to this URI after the `frontChannelLogoutUris` have been called. This variable can be empty if the value cannot be defined. |

#### Available messages

| Key                                        | Description                                        | Note                                                                                       |
|--------------------------------------------|----------------------------------------------------|--------------------------------------------------------------------------------------------|
| pages.openid.endsession.success.title      | Page title to be used in the browser title.        |                                                                                            |
| openid.endsession.success.header           | Title of the page in the body.                     |                                                                                            |
| openid.endsession.success.goToRelyingParty | Paragraph that the user is redirected to the RP.   | HTML is allowed. The postLogoutRedirectUri is added as a parameter in the default message. |

### End session reject

This is a dead end page after the user has rejected the logout in the confirm screen. At this point it is unknown which RP had initiated the logout.

#### Naming convention

The name of the template file must be `endsession-reject.html`.

#### Available messages

| Key                                        | Description                                        | Note                                                               |
|--------------------------------------------|----------------------------------------------------|--------------------------------------------------------------------|
| pages.openid.endsession.reject.title       | Page title to be used in the browser title.        |                                                                    |
| openid.endsession.reject.header            | Title of the page in the body.                     |                                                                    |
| openid.endsession.reject.body              | Paragraph of text.                                 | HTML is allowed.                                                   |