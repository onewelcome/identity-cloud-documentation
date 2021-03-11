# Onegini Access audit events

## Uncategorized audit events

| Event                                                         | Description                                                                                                                                                                                                           |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CONSENT GIVEN                                                 | User has given consent for one or more scopes for a client.                                                                                                                                                           |
| CSRF INVALID                                                  | The provided CSRF token is invalid.                                                                                                                                                                                   |
| API AUTHENTICATION FAILED                                     | Invalid username or password provided while accessing the end user API. The credentials must be configured via an [API client](./administration/api-config.md).                                                      |
| API CONSENT LISTED                                            | A list of consents was returned for a user via the end user API.                                                                                                                                                      |
| API CONSENT NO DATA FOUND                                     | No consents could be found for a user via the end user API. This means that the user has not given consent before or they have revoked their consent.                                                               |
| API CONSENT REMOVED                                           | Consent for a client (all scopes) has been removed for a user via the end user API. The user has to give consent again when they request an access token.                                                           |
| API CONSENT NOT REMOVED NOT FOUND                             | Unable to remove consent via the end user API because no corresponding consent could be found.                                                                                                                        |
| API CONSENT NOT REMOVED FORBIDDEN                             | Unable to remove a consent via the end user API because the consent requested for removal does not belong to the specified user.                                                                                      |
| API TOKENS LISTED                                             | A list of access tokens was returned for a user via the end user API.                                                                                                                                                 |
| API TOKENS NO DATA FOUND                                      | No access tokens could be found for a user via the end user API. This can occur when the user has never retrieved an access token or when all their tokens have been revoked.                                        |
| API TOKEN REMOVED                                             | Access token for a client has been removed for a user via the end user API.                                                                                                                                           |
| API TOKEN NOT REMOVED NOT FOUND                               | Unable to remove an access token via the end user API because no corresponding access token could be found.                                                                                                        |
| API TOKEN NOT REMOVED FORBIDDEN                               | Unable to remove a token via the end user API because the token requested for removal does not belong to the specified user.                                                                                          |
| API DEVICES LISTED                                            | A list of devices was returned for a user via the end user API.                                                                                                                                                       |
| API DEVICES NO DATA FOUND                                     | No devices could be found for a user via the end user API.                                                                                                                                                            |
| API DEVICE REMOVED                                            | Device for a user is removed via the end user API.                                                                                                                                                                    |
| API DEVICE REMOVAL FAILED                                     | An attempt to remove device for a user via end user API failed.                                                                                                                                                       |
| API DEVICE REMOVED FOR ONE PROFILE                            | Device data related to one user profile was removed via the end user API.                                                                                                                                             |
| API DEVICE REMOVAL FOR ONE PROFILE FAILED                     | An attempt to remove device data related to one user profile via the end user API failed.                                                                                                                             |
| API DEVICE NOT REMOVED FORBIDDEN                              | Unable to remove a device via the end user API because the device requested for removal does not belong to the specified user.                                                                                        |
| API DEVICE NOT REMOVED NOT FOUND                              | Unable to remove a device via the end user API because client wasn't found.                                                                                                                                            |
| API FINGER PRINT NOT DISABLED CLIENT NOT FOUND                | Unable to disable finger print via the end user API because no corresponding device could be found.                                                                                                                   |
| API FINGER PRINT NOT DISABLED FORBIDDEN                       | Unable to disable finger print via the end user API because no the device requested does not belong to the specified user.                                                                                            |
| API FINGER PRINT DISABLED                                     | Finger print was disabled for the specified user and device.                                                                                                                                                          |
| API APP LISTED                                                | A list of applications was returned for a user via the end user API                                                                                                                                                   |
| API APP NO DATA FOUND                                         | No applications could be found for a user via the end user API                                                                                                                                                        |
| API APP REMOVED                                               | Access to an application for a user is removed via the end user API                                                                                                                                                   |
| API APP NOT REMOVED NOT FOUND                                 | No applications to remove could be found for a user via the end user API                                                                                                                                              |
| API AUTHENTICATION APPS LISTED                                | A list of applications enrolled for push authentication was returned for a user via the end user API.                                                                                                                                  |
| API AUTHENTICATION APP NOT FOUND                              | No application enrolled for push authentication could be found for a user via the end user API.                                                                                                                                       |
| API AUTHENTICATION APP REMOVED                                | The push authentication capabilities for an app were removed for a user via the end user API.                                                                                                                         |
| API AUTHENTICATION APP NOT REMOVED NOT FOUND                  | The push authentication capabilities for an app could not be removed because push enrollment could not be found via the end user API.                                                                                    |
| API AUTHENTICATION APP NOT REMOVED FORBIDDEN                  | The push authentication capabilities for an app could not be removed because push enrollment belongs to other user.                                                                                                      |
| API AUTHENTICATION APP AND USER CLIENT KEYS REMOVED           | The mobile authentication capabilities for a client were removed for a user via the end user API.                                                                                                                     |
| API AUTHENTICATION USER CLIENT KEYS NOT REMOVED NOT FOUND     | The mobile authentication capabilities for a client could not be removed because user client keys could not be found via the end user API.                                                                            |
| API AUTHENTICATION USER CLIENT KEYS NOT REMOVED FORBIDDEN     | The mobile authentication capabilities for a client could not be removed because user client keys belongs to other user.                                                                                              |
| API PAYLOAD ENCRYPTION APPLICATION DISABLED                   | App version is disabled.                                                                                                                                                                                              |
| API PAYLOAD ENCRYPTION POLICY NOT FOUND                       | No payload encryption policy could be found via the the API endpoint.                                                                                                                                                 |
| API PAYLOAD ENCRYPTION INVALID PARAMETERS                     | Invalid parameters were forwarded to the API endpoint.                                                                                                                                                                |
| API PAYLOAD ENCRYPTION POLICY FOUND                           | A payload encryption policy was returned for the Onegini Security Proxy via the API endpoint.                                                                                                                                   |
| API PAYLOAD ENCRYPTION INVALID ACCESS TOKEN                   | Access token could not be found or was invalid.                                                                                                                                                                       |
| API PAYLOAD ENCRYPTION INVALID CLIENT ASSERTION               | Failed to resolve or authenticate the client based on the provided client assertion.                                                                                                                                  |
| API PAYLOAD ENCRYPTION INVALID CLIENT                         | The given client is not allowed to use this version of Payload Encryption API.                                                                                                                                            |
| API PAYLOAD ENCRYPTION NOT SUPPORTED BY CLIENT                | The given client does not support the requested version of the Payload Encryption.                                                                                                                                                      |
| API VERSION NOT ALLOWED                                       | The used endpoint is not allowed for the api version used by the client.                                                                                                                                              |
| API EVENTS LISTED                                           | A list of events was returned via the API.                                                                                            |
| EXTERNAL SCOPE CHECK FAILED                                   | An error occurred while performing the external scope check via the scope service endpoint.                                                                                                                           |
| ADMIN AUTHENTICATION FAILURE                                  | User has entered an incorrect username and/or password to log in into the admin console.                                                                                                                              |
| ADMIN AUTHENTICATION SUCCESS                                  | User has successfully logged into the admin console. The details provide information about the roles that the user has in the admin console. No roles means that he does not have any permissions.                    |
| ADMIN CLIENT DELETED                                          | An oauth client has been deleted via the admin console.                                                                                                                                                               |
| ADMIN CLIENT DELETION FAILURE                                 | An attempt to delete an oauth client via the admin console failed.                                                                                                                                                    |
| ADMIN CLIENT UPDATED                                          | An oauth client has been updated via the admin console.                                                                                                                                                               |
| ADMIN CLIENT CONFIG DELETED                                   | Admin client configuration was successfully deleted.                                                                                                                                                                  |
| ADMIN STATIC CLIENT CREATED                                   | A static oauth client has been created via the admin console.                                                                                                                                                         |
| ADMIN API CLIENT CREATED                                      | An oauth client for API calls has been created via the admin console.                                                                                                                                                 |
| ADMIN OPENID CLIENT CONFIG CREATED                            | An OpenID configuration for a client has been created via the admin console                                                                                                                                           |
| ADMIN OPENID CLIENT CONFIG DELETED                            | An OpenID configuration for a client has been deleted via the admin console                                                                                                                                           |
| ADMIN OPENID CLIENT CONFIG UPDATED                            | An OpenID configuration for a cleint has been updated via the admin console                                                                                                                                           | 
| ADMIN SCOPE DELETED                                           | An oauth scope has been deleted via the admin console.                                                                                                                                                                |
| ADMIN SCOPE CREATED                                           | An oauth scope has been created via the admin console.                                                                                                                                                                |
| ADMIN SCOPE UPDATED                                           | An oauth scope has been updated via the admin console.                                                                                                                                                                |
| ADMIN MOBILE APP CREATED                                      | A mobile application has been created via the admin console.                                                                                                                                                          |
| ADMIN MOBILE APP DELETED                                      | A mobile application has been deleted via the admin console.                                                                                                                                                          |
| ADMIN MOBILE APP UPDATED                                      | A mobile application has been updated via the admin console.                                                                                                                                                          |
| ADMIN MOBILE PLATFORM CREATED                                 | A mobile platform has been created via the admin console.                                                                                                                                                             |
| ADMIN MOBILE PLATFORM DELETED                                 | A mobile platform has been deleted via the admin console.                                                                                                                                                             |
| ADMIN MOBILE PLATFORM UPDATED                                 | A mobile platform has been updated via the admin console.                                                                                                                                                             |
| ADMIN MOBILE CONFIG UPDATED                                   | The configuration for mobile authentication has been changed in the admin console.                                                                                                                                    |
| ADMIN CORS CONFIG UPDATED                                     | The configuration for CORS has been changed in the admin console.                                                                                                                                                     |
| ADMIN SYSTEM EXTENSION ENGINE CONFIG UPDATED                  | The configuration for the connection to the Onegini Extension Engine has been changed in the admin console.                                                                                                                           |
| ADMIN SYSTEM EXTENSION ENGINE PROPERTY CREATED                | A property in the Onegini Extension Engine has been created via the admin console.                                                                                                                                                     |
| ADMIN SYSTEM EXTENSION ENGINE PROPERTY DELETED                | A property in the Onegini Extension Engine has been deleted via the admin console.                                                                                                                                                     |
| ADMIN SYSTEM EXTENSION ENGINE PROPERTY UPDATED                | A property in the Onegini Extension Engine has been updated via the admin console.                                                                                                                                                     |
| ADMIN SYSTEM FEATURE CONFIG UPDATED                           | The configuration for system features has been changed in the admin console.                                                                                                                                          |
| ADMIN SYSTEM CACHE CONFIG UPDATED                             | The configuration for system cache configuration has been changed in the admin console.
| ADMIN SYSTEM GENERAL CONFIG UPDATED                           | The configuration for system general configuration has been changed in the admin console.
| ADMIN SYSTEM GEOLOCATION CONFIG UPDATED                       | The configuration for the geolocation service has been changed in the admin console.
| ADMIN SYSTEM AWS CONFIG UPDATED                               | The AWS configuration has been changed in the admin console.
| ADMIN MOBILE PLATFORM VERSION CREATED                         | An App version has been created for a mobile application via the admin console.                                                                                                                                       |
| ADMIN MOBILE PLATFORM VERSION UPDATED                         | An App version has been updated for a mobile application via the admin console.                                                                                                                                       |
| ADMIN MOBILE PLATFORM VERSION DELETED                         | An App version has been deleted for a mobile application via the admin console.                                                                                                                                       |
| ADMIN AUTH PROPERTIES SET CREATED                             | Mobile authentication type has been created via the admin console.                                                                                                                                                    |
| ADMIN AUTH PROPERTIES SET UPDATED                             | Mobile authentication type has been updated via the admin console.                                                                                                                                                    |
| ADMIN AUTH PROPERTIES SET DELETED                             | Mobile authentication type has been deleted via the admin console.                                                                                                                                                    |
| ADMIN CALLBACK CONFIG CREATED                                 | Callback configuration has been created via the admin console.                                                                                                                                                        |
| ADMIN CALLBACK CONFIG UPDATED                                 | Callback configuration has been updated via the admin console.                                                                                                                                                        |
| ADMIN CALLBACK CONFIG DELETED                                 | Callback configuration has been deleted via the admin console.                                                                                                                                                        |
| ADMIN PUSH MESSAGING CONFIG CREATED                           | Push messaging configuration has been created via the admin console.                                                                                                                                                  |
| ADMIN PUSH MESSAGING CONFIG UPDATED                           | Push messaging configuration has been updated via the admin console.                                                                                                                                                  |
| ADMIN PUSH MESSAGING CONFIG DELETED                           | Push messaging configuration has been deleted via the admin console.                                                                                                                                                  |
| ADMIN IDP SAML SP CONFIG UPDATED                              | The configuration for SAML Service Provider has been changed in the admin console.                                                                                                                                    |
| ADMIN IDENTITY PROVIDER CREATED                               | An identity provider has been created via the admin console.                                                                                                                                                          |
| ADMIN IDENTITY PROVIDER DELETED                               | An identity provider has been deleted via the admin console.                                                                                                                                                          |
| ADMIN IDENTITY PROVIDER UPDATED                               | An identity provider has been updated via the admin console.                                                                                                                                                          |
| ADMIN IDENTITY PROVIDER ATTRIBUTE MAPPING CREATED             | An attribute mapping has been created for an identity provider via the admin console.                                                                                                                                 |
| ADMIN IDENTITY PROVIDER ATTRIBUTE MAPPING DELETED             | An attribute mapping has been deleted for an identity provider via the admin console.                                                                                                                                 |
| ADMIN IDENTITY PROVIDER ATTRIBUTE MAPPING UPDATED             | An attribute mapping has been updated for an identity provider via the admin console.                                                                                                                                 |
| ADMIN PIN POLICY CREATED                                      | A pin policy has been created via the admin console.                                                                                                                                                                  |
| ADMIN PIN POLICY DELETED                                      | A pin policy has been deleted via the admin console.                                                                                                                                                                  |
| ADMIN PIN POLICY UPDATED                                      | A pin policy has been updated via the admin console.                                                                                                                                                                  |
| ADMIN USER REVOKED                                            | Revoke user from a device via the admin console.                                                                                                                                                                      |
| ADMIN USER REVOKE FAILURE                                     | An attempt to revoke user from device via the admin console failed.                                                                                                                                                   |
| ADMIN PUSH ENROLLMENT REVOKED                                 | Revoke push enrollment for a user via the admin console.                                                                                                                                                              |
| ADMIN PUSH ENROLLMENT NOT REVOKED NOT FOUND                   | Attempt to revoke push enrollment for a user via the admin console, but no push enrollment was found.                                                                                                                 |
| ADMIN PUSH_ENROLLMENT NOT REVOKED INCORRECT DEVICE            | Attempt to revoke push enrollment for a user via the admin console, but the device does not match or is unknown.                                                                                                      |
| ADMIN USER CLIENT KEYS REVOKED                                | Revoke mobile authentication enrollment for a user via the admin console.                                                                                                                                             |
| ADMIN USER CLIENT KEYS NOT REVOKED NOT FOUND                  | Attempt to revoke mobile authentication enrollment for a user via the admin console, but no mobile authentication enrollment was found.                                                                               |
| ADMIN USER CLIENT KEYS NOT REVOKED INCORRECT DEVICE           | Attempt to revoke mobile authentication enrollment for a user via the admin console, but the device does not match or is unknown.                                                                                     |
| ADMIN CUSTOM AUTHENTICATOR CREATED                            | Configuration for a Custom Authenticator has been added via the admin console.                                                                                                                                        |
| ADMIN CUSTOM AUTHENTICATOR UPDATED                            | Configuration for a Custom Authenticator has been modified via the admin console.                                                                                                                                     |
| ADMIN CUSTOM AUTHENTICATOR DELETED                            | Configuration for a Custom Authenticator has been removed via the admin console.                                                                                                                                      |
| ADMIN DUM ENGINE CONFIG CREATED                               | Delegated Administration configuration has been added via the admin console.                                                                                                                                        |
| ADMIN DUM ENGINE CONFIG UPDATED                               | Delegated Administration configuration has been modified via the admin console.                                                                                                                                     |
| ADMIN DUM ENGINE CONFIG DELETED                               | Delegated Administration configuration has been removed via the admin console.                                                                                                                                      |
| HELPDESK CLIENT DELETE UNAUTHORIZED                           | Attempt to delete a dynamic client via the admin console, but this operation is not permitted for the helpdesk user.                                                                                                  |
| HELPDESK MOBILE AUTHENTICATION ENROLLMENT REVOKE UNAUTHORIZED | Attempt to revoke mobile authentication for a user via the admin console, but this operation is not permitted for the helpdesk user.                                                                                  |
| HELPDESK APPLICATION INSTANCE NOT REVOKED FORBIDDEN           | This event is renamed to "HELPDESK APPLICATION INSTANCE REVOKE UNAUTHORIZED".                                                                                                                                         |
| HELPDESK USER REVOKE UNAUTHORIZED                             | Attempt to disconnect a user from a device via the admin console, but this operation is not permitted for the helpdesk user.                                                                                          |
| CLIENT CREDENTIALS CHECK SUCCESS                              | Successfully verified the client credentials                                                                                                                                                                          |
| CLIENT CREDENTIALS CHECK SKIPPED                              | The client credentials are not validated because the application is running in development mode.                                                                                                                      |
| CLIENT CREDENTIALS CHECK FAILED                               | Could not verify the client credentials                                                                                                                                                                               |
| CLIENT CREDENTIALS CHECK FAILED                               | Client credentials validation request is invalid                                                                                                                                                                      |
| CLIENT AUTHENTICATION FAILED INVALID JWKS URI                 | The jwksUri was not reachable or did not return a valid response.                                                                                                                                                     | 
| AUTHORIZATION LVL UNSUPPORTED                                 | The authentication level specified in the authentication level header of the incoming request can not be parsed to a valid integer value.                                                                             |
| AUTHORIZATION FAILED UNABLE TO GENERATE ID TOKEN              | Error occurred during id token generation process, check OpenID Connect signing/encryption properties.                                                                                                                |
| AUTHZ REQUEST TRANSACTION TAMPERING                           | The identifier of client or IdP in the request is different that the one in the transaction                                                                                                                           |
| AUTHZ REQUEST AUTHENTICATION LVL INSUFFICIENT                 | The authentication level specified in the authentication level header of the incoming request is lower then the required authentication level by one or more of the requested scopes.                                 |
| AUTHZ REQUEST INVALID                                         | The OAuth authorization request is missing one or more required fields.                                                                                                                                               |
| AUTHZ REQUEST REDIRECT URI INVALID                            | The provided redirect uri in the OAuth authorization request does not match the configured redirect uri for the specified client.                                                                                     |
| AUTHZ REQUEST SCOPE INVALID                                   | One or more of the requested scopes is not available for the specified client.                                                                                                                                        |
| AUTHZ REQUEST NOT AUTHORIZED FOR SCOPE                        | The user is not authorized for the requested scope. See the [scope verification service](../topics/integration-extension/scope-verification/scope-verification.md) topic guide for more info.                         |
| AUTHZ REQUEST GRANT CREATED                                   | An authorization code is created for the requested client and scopes.                                                                                                                                                 |
| AUTHZ REQUEST INVALID CLIENT                                  | The `client_id` in the OAuth authorization request is invalid or unknown                                                                                                                                              |
| AUTHZ REQUEST INVALID RESPONSE TYPE                           | The response type is not supported by the application. The Onegini Access only supports `response_type=code`.                                                                                                   |
| AUTHZ REQUEST UNAUTHORIZED CLIENT                             | The client is not allowed to perform this action. Make sure the 'authorization code' flow is enabled for a used web client or 'user registration' for a mobile application.                                          |
| AUTHN REQUEST LOGIN REQUIRED                                  | The client has requested authentication without user interaction, but the user has no session or the identity provider does not support authentication without user interaction.                                      |
| AUTHN REQUEST CONSENT REQUIRED                                | The client has requested authentication without user interaction, but the user has to give consent.                                                                                                                   |
| TOKEN REQUEST INVALID                                         | The access token request is missing one or more required fields.                                                                                                                                                      |
| TOKEN REQUEST INVALID REDIRECT URI                            | The provided redirect uri in the access token request does not match the configured redirect uri for the specified client.                                                                                            |
| TOKEN REQUEST INVALID GRANT                                   | The access grant used to request an access token was expired or revoked.                                                                                                                                              |
| TOKEN REQUEST SCOPE INVALID                                   | One or more of the requested scopes is not available for the specified client.                                                                                                                                        |
| TOKEN REQUEST NOT AUTHORIZED FOR SCOPE                        | The user does not have a product for the requested scope.                                                                                                                                                             |
| TOKEN REQUEST INVALID CLIENT CREDENTIALS                      | Provided client id and client secret do not match.                                                                                                                                                                    |
| TOKEN REQUEST INVALID REFRESH TOKEN                           | No access token could be found based on the provided refresh token.                                                                                                                                                   |
| TOKEN REQUEST INVALID BEARER TOKEN                            | No access token could be found based on the provided bearer token.                                                                                                                                                    |
| TOKEN REQUEST DISABLED GRANT TYPE                             | The required grant type is not enabled for the client.                                                                                                                                                                |
| TOKEN REQUEST UNSUPPORTED GRANT TYPE                          | Grant type used is not recognized and supported by the application.                                                                                                                                                   |
| TOKEN REQUEST INVALID CLIENT                                  | No client could be found based on the specified client id.                                                                                                                                                            |
| TOKEN REQUEST ACCESS TOKEN CREATED                            | Access token created via provided authorization code.                                                                                                                                                                 |
| TOKEN REQUEST ACCESS TOKEN REFRESHED                          | A new access token is created based on the provided refresh token.                                                                                                                                                    |
| TOKEN REQUEST INVALID ACCESS TOKEN                            | The provided access token (for validation) is expired or revoked.                                                                                                                                                     |
| TOKEN REQUEST ACCESS TOKEN VALIDATED                          | The provided access token (for validation) is valid.                                                                                                                                                                  |
| TOKEN REQUEST FINGER PRINT INVALID REQUEST                    | The refresh token request is missing one or more required fields.
| TOKEN REQUEST FINGER PRINT INVALID CLIENT CREDENTIALS         | The client credentials used to fetch a finger print refresh token were invalid.                                                                                                                                       |
| TOKEN REQUEST FINGER PRINT INVALID REFRESH TOKEN              | The refresh token used to fetch a finger print refresh token was invalid.                                                                                                                                             |
| TOKEN REQUEST FINGER PRINT TOKEN CREATED                      | A refresh token for finger print usage was created and the original refresh token was refreshed.                                                                                                                      |
| TOKEN REQUEST FINGER PRINT ACCESS TOKEN REFRESHED             | The finger print refresh token was refreshed                                                                                                                                                                          |
| TOKEN REQUEST FINGER PRINT REFRESH TOKEN RETRY COUNT EXCEEDED | The finger print refresh token used was wrong, therefore all finger print refresh tokens for this client are removed                                                                                                  |
| TOKEN REQUEST INVALID PROFILE ID                              | Provided profile id has invalid format or is already used by other user of the client.                                                                                                                                |
| TOKEN REQUEST IMPLICIT AUTHENTICATION ACCESS TOKEN CREATED    | An implicit authentication access token was created based on the provided client credentials, profile id and scopes.                                                                                                  |
| TOKEN REQUEST REFRESH TOKEN RETRY COUNT EXCEEDED              | The refresh token has been used wrongly for more than the maximum allowed retries. The refresh token is invalidated.                                                                                                  |
| TOKEN RESPONSE GROUP PERMISSIONS EXCEEDED JWT LIMIT           | The JWT access token with the claim [`group permissions`](../topics/tokens/access-token.md#group-permissions) has exceeded the configured length of a JWT access token. |
| TOKEN REVOKE INVALID REQUEST                                  | Invalid request while revoking token (ie. malformed syntax, required parameter missing).                                                                                                                              |
| TOKEN REVOKE ACCESS TOKEN SUCCESS                             | The access token was successfully revoked, corresponding refresh token when available is not revoked.                                                                                                                 |
| TOKEN REVOKE ACCESS TOKEN INVALID                             | The access token could not be revoked.                                                                                                                                                                                |
| TOKEN REVOKE REFRESH TOKEN SUCCESS                            | The refresh token was successfully revoked including the access tokens based on this refresh token.                                                                                                                   |
| TOKEN REVOKE REFRESH TOKEN INVALID                            | The refresh token could not be revoked.                                                                                                                                                                               |
| TOKEN REVOKE CLIENT INVALID                                   | The token could not be revoked because the client is not found or the client credentials are invalid.                                                                                                                 |
| TOKEN REVOKE TOKEN INVALID                                    | The token could not be revoked because it is not found. There was no token type hint specified in the request.                                                                                                        |
| TOKEN REVOKE CLIENT TOKENS SUCCESS                            | All tokens available for a client were removed via the client tokens revocation endpoint.                                                                                                                             |
| TOKEN REVOKE PROFILE TOKENS SUCCESS                           | All tokens available for a profile were removed via the profile revocation endpoint.                                                                                                                                  |
| TOKEN REVOKE CLIENT FINGER PRINT TOKENS SUCCESS               | All fingerprint tokens available for a client were removed via the token revocation endpoint.                                                                                                                         |
| TOKEN REVOKE PROFILE TOKENS SUCCESS                           | All tokens available for a profile on a client were revoked using the profile token revocation endpoint.                                                                                                              |
| TOKEN REVOKE PROFILE FINGER PRINT TOKENS SUCCESS              | All fingerprint tokens available for a profile on a client were removed using the profile token revocation endpoint.                                                                                                  |
| TOKEN INTROSPECTION SUCCESS                                   | Token introspection of the token was successful                                                                                                                                                                       |
| TOKEN INTROSPECTION FAILED                                    | Token introspection failed, access token used is expired or revoked.                                                                                                                                                  |
| ONE TIME PASSWORD AUTH SUCCEEDED                              | Successfully authenticated using One Time Password.                                                                                                                                                                   |
| ONE TIME PASSWORD AUTH FAILED                                 | One Time Password authentication failed.                                                                                                                                                                              |
| ONE TIME PASSWORD GENERATION FAILED MISSING USER ID           | Failed to generate One Time Password due to a missing user identifier.                                                                                                                                                |
| API ACCESS NOT ALLOWED INVALID CREDENTIALS                    | The client credentials provided to access the API were invalid.                                                                                                                                                       |
| API ACCESS NOT ALLOWED DISABLED GRANT TYPE                    | The required grant type to access the API is not enabled for the client.                                                                                                                                              |
| API ACCESS NOT ALLOWED INVALID SCOPES                         | The required scope to access the API is not enabled for the client.                                                                                                                                                   |
| API ACCESS GRANTED                                            | Access to the API was successfully granted.                                                                                                                                                                           |
| UNABLE TO DETERMINE IDENTITY PROVIDER                         | No identity provider could be selected for the incoming authentication request.                                                                                                                                       |
| IDENTITY PROVIDER UNSUPPORTED                                 | This identity provider is not supported. Possible scenario is when an API based identity provider is used for a browser based authentication flow.                                                                    |
| MOBILE CLIENT UPDATED PLATFORM VERSION                        | Updated the App version for a client                                                                                                                                                                                  |
| SMS STEP UP CSRF INVALID                                      | The CSRF token provided while submitting a sms code is invalid. A CSRF token is only valid for a limited amount of time. All details in the request should match the values expected for this particular CSRF token.  |
| SMS STEP UP CODE INVALID                                      | The code submitted during sms step up was invalid.                                                                                                                                                                    |
| IDP OAUTH STATE MISMATCH                                      | The expected value for the state parameter from the OAuth IdP does not match the stored value, or one of the state values is missing.                                                                                 |
| IDP OAUTH MISSING ACCESS GRANT                                | The OAuth IdP did not return an access grant.                                                                                                                                                                         |
| IDP OAUTH MISSING ACCESS TOKEN                                | The OAuth IdP did not return an access token.                                                                                                                                                                         |
| IDP OAUTH MISSING USER INFO                                   | The OAuth IdP did not return user information or the response was in an unexpected format.                                                                                                                            |
| CONFIG BOOTSTRAP API CLIENT CREATED                           | The API client has been created during startup. Its credentials have been configured via environment variables.                                                                                                       |
| ONEGINI IDP EXTERNAL IDPS FETCH FAILED                        | Onegini Access was unable to fetch the list of external identity providers that are configured in the [Consumer Identity Manager](https://docs-single-tenant.onegini.com/cim/stable/idp) application.                |

## Dynamic clients

The following events are specific for mobile apps that use the Onegini SDK.

### Client registration events

| Event                                                         | Description
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------
| CLIENT MIGRATION SUCCESS                                      | The client has been successfully migrated to the newest version of security protocol.
| CLIENT MIGRATION FAILED OS VERSION INVALID                    | Migration failed, the client's OS version is blocked or not supported.
| CLIENT MIGRATION FAILED VERSION DISABLED                      | Migration failed, the requested mobile app version is disabled or does not allow new registrations.
| CLIENT MIGRATION FAILED VERSION UNCHANGED                     | Migration failed, the client is already associated with the given app platform version.
| CLIENT MIGRATION FAILED APP CLIENT MISMATCH                   | Migration failed, the requested mobile application does not match with the client's configuration.
| CLIENT MIGRATION FAILED INVALID SOFTWARE STATEMENT            | Migration failed, the software statement is invalid (ie. cannot be parsed or signature verification failed.
| CLIENT MIGRATION FAILED PLATFORM VERSION UNKNOWN              | Migration failed, the client's platform version is unknown.
| CLIENT MIGRATION FAILED INVALID METADATA                      | Migration failed, the client metadata is not supported by the server.
| CLIENT MIGRATION FAILED INVALID RESPONSE                      | Migration failed, the response to the given challenge was incorrect.
| DYNAMIC REG SUCCESS                                           | A new client was registered successfully via dynamic client registration.
| DYNAMIC REG FAILED                                            | Dynamic client registration failed because the challenge response did not match the expected value.
| DYNAMIC REG FAILED REGISTRATION ID ALREADY EXISTS             | Dynamic client registration failed, because the registration id has been used before.
| DYNAMIC REG FAILED REGISTRATION ID INVALID FORMAT             | Dynamic client registration failed, because the registration id is missing or does not match the expected format.
| DYNAMIC REG INVALID HEADER                                    | Unable to perform dynamic client registration because one or more required headers is missing or no mobile application can be found based on the header values.
| DYNAMIC REG INVALID REQUEST                                   | Unable to perform dynamic client registration because identifier or challenge response is not available in request.
| DYNAMIC REG NEW REGISTRATIONS DISABLED                        | Dynamic client registration failed because new registrations are disabled for this platform version.
| DYNAMIC REG OS VERSION INVALID                                | Dynamic client registration failed because client's OS version is blocked or not supported.
| DYNAMIC REG PLATFORM VERSION UNKNOWN                          | Dynamic client registration failed because the client's platform version is unknown.
| DYNAMIC REG VERSION DISABLED                                  | Dynamic client registration failed because the platform version is disabled
| DYNAMIC REG FAILED INVALID SOFTWARE STATEMENT                 | Dynamic client registration failed because the software statement is invalid (ie. cannot be parsed or signature verification failed
| DYNAMIC UP SUCCESS                                            | Dynamic client upgrade was successfully executed. The application version associated with the dynamic client has been updated.
| DYNAMIC UP FAILED                                             | Dynamic client upgrade failed.
| DYNAMIC REG FAILED INVALID METADATA                           | Dynamic client registration failed because the client metadata is not supported by the server
| DYNAMIC REG FAILED INVALID RESPONSE                           | Dynamic client registration failed because response to the given challenge was incorrect

### Client validation events

Client validation happens at start up of a mobile app with the Onegini SDK. The app validates whether its registration is still valid and receives up to date
configuration.

| Event                                                         | Description
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------
| CLIENT VALIDATION SUCCESS                                     | Client validation was successful.
| CLIENT VALIDATION ABUSE DETECTED                              | Client validation failed because abuse was detected. Verify the application signature.
| CLIENT VALIDATION DEBUGGER DETECTED                           | Client validation failed because debugger was detected.
| CLIENT VALIDATION FAILURE MOBILE APP MISMATCH                 | Client validation failed because the requested app platform version in the validation request does not match the app platform version resolved from the client authentication.
| CLIENT VALIDATION INVALID CLIENT                              | Client validation failed because client wasn't found, invalid credentials or configuration was provided.
| CLIENT VALIDATION INVALID CLIENT TYPE                         | Client validation failed because client type is invalid.
| CLIENT VALIDATION INVALID HEADER                              | Client validation failed because one or more required headers are missing or no mobile application can be found based on the header values.
| CLIENT VALIDATION INVALID SCOPE                               | Client validation failed because scope is invalid.
| CLIENT VALIDATION JAILBREAK DETECTED                          | Client validation failed because jailbreak was detected.
| CLIENT VALIDATION NO CLIENT CREDENTIALS GRANT                 | Client validation failed because client has no client credentials grant enabled.
| CLIENT VALIDATION OS VERSION INVALID                          | Client validation failed because client's OS version is blocked or not supported.
| CLIENT VALIDATION UPGRADE INITIALIZED                         | Client validation resulted in the initialization of Dynamic Client Upgrade.
| CLIENT VALIDATION VERSION DISABLED                            | Client validation failed because the version has been disabled.
| CLIENT VALIDATION PLATFORM VERSION UNKNOWN                    | Client validation failed because the client's platform version is unknown
| CLIENT VALIDATION INVALID SOFTWARE STATEMENT                  | Client validation failed because the software statement is invalid (ie. cannot be parsed or signature verification failed
| CLIENT VALIDATION VERSION UPDATED SUCCESSFULLY                | Application version successfully updated during client validation.

## JWT Key events

| Event                                                       | Description
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------
| JWT KEYS ROTATED                                            | The keys for the Json Web Token signing were rotated
| JWT KEYS ALGORITHM UPDATED                                  | The algorithm for the Json Web Token signing was updated. Keys will also have changed since the algorithm was modified.

## OpenID Connect events

| Event                                                       | Description
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------
| ID TOKEN CREATED                                            | ID Token was created.
| ID TOKEN INVALID CONFIGURATION                              | Failed to create ID Token due to invalid OpenID Connect configuration.
| OPENID SESSION LOGOUT SUCCESS                               | The OpenID session was ended and all clients were logged out.
| OPENID SESSION LOGOUT CONFIRM PAGE SHOWN                    | A confirm page is shown when trying to end the OpenID session.
| OPENID SESSION LOGOUT FAILURE REJECTED                      | An attempt to end OpenID session failed because the user has rejected the confirmation.
| OPENID SESSION LOGOUT FAILURE                               | An attempt to end OpenID session failed.
| OPENID TOKEN ENCRYPTION ALGORITHM NOT SUPPORTED             | The encryption algorithm parsed from the Relying Party's JWK is not supported by Onegini Access.
| OPENID TOKEN ENCRYPTION METHOD NOT SUPPORTED                | The configured encryption method is not supported by Onegini Access.
| OPENID TOKEN ENCRYPTION KEY TYPE NOT SUPPORTED              | JWKSet (response from Relying party server) does not contain a supported type of key.
| OPENID TOKEN ENCRYPTION BAD JWKS URI                        | The jwks_uri was not reachable or did not return a proper response.
| OPENID TOKEN ENCRYPTION FAILED                              | Failed to encrypt a signed JWT.

## Mobile authentication

This section contains events for mobile authentication

### Generic enrollment audit events

| Event                                                         | Description
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------
| ENROLLMENT FAILURE DISABLED                                   | Failed to process the enrollment request because enrollment is disabled.
| ENROLLMENT INITIALIZED                                        | Successfully initialized the enrollment process.
| ENROLLMENT INIT FAILURE INVALID REQ                           | Failed to initialize enrollment because input params were invalid.
| ENROLLMENT INIT FAILURE INVALID TOKEN                         | Failed to initialize enrollment because the access token was invalid.
| ENROLLMENT KEY FETCHED                                        | Successfully fetched the public key for the mobile authentication server.
| ENROLLMENT KEY FAILURE INVALID ID                             | Failed to generate the public key because the given identifier was not found.
| ENROLLMENT ACK SUCCESS                                        | Successfully acknowledged the enrollment.
| ENROLLMENT KEY FAILURE INVALID REQ                            | Failed to generate the public key because input params were invalid.
| ENROLLMENT ACK FAILURE INVALID ID                             | Failed to acknowledge the enrollment because the given identifier was not found.
| ENROLLMENT UPDATE SUCCESS                                     | Successfully updated the push token.
| ENROLLMENT UPDATE FAILURE INVALID REQ                         | Failed to update the push token because input params were invalid or the old push token provided did not match the client's current push token.
| ENROLLMENT UPDATE FAILURE INVALID TOKEN                       | Failed to update the push token because the access token was invalid.
| ENROLLMENT UPDATE FAILURE UNSUPPORTED PLATFORM                | Failed to update the push token because the client's platform is not supported by this operation.

### Key enrollment audit events

| Event                                  | Description                                                                                                       |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| KEY ENROLLMENT SUCCESS                 | Successfully enrolled keys for a user.                                                                            |
| KEY ENROLLMENT FAILURE INVALID REQUEST | Failed to process the key enrollment request because input parameters were invalid.                               |
| KEY ENROLLMENT FAILURE DISABLED        | Failed to process the key enrollment request because the key enrollment is disabled.                              |
| KEY ENROLLMENT FAILURE UPDATE DISABLED | Failed to perform key enrollment because the update of the keys for the key enrollment is disabled. (deprecated)  |
| KEY ENROLLMENT FAILURE INVALID TOKEN   | Failed to perform key enrollment because the access token was invalid.                                            |

### Push enrollment audit events

| Event                                              | Description
|----------------------------------------------------|---------------------------------------------------------------------------------------------------------------
| PUSH ENROLLMENT SUCCESS                            | The user was successfully enrolled for mobile authentication with push.
| PUSH ENROLLMENT FAILURE FEATURE DISABLED           | Failed to enroll for push because the feature is disabled.
| PUSH ENROLLMENT FAILURE INVALID REQ                | Failed to enroll for push because the request was malformed or had missing parameters.
| PUSH ENROLLMENT FAILURE INVALID ACCESS TOKEN       | Failed to enroll for push because the access token that was provided was invalid.
| PUSH ENROLLMENT FAILURE KEYS NOT EXCHANGED         | Failed to enroll for push because the user has not yet performed key enrollment (required for push).
| PUSH ENROLLMENT FAILURE EXISTING PUSH TOKEN        | Failed to enroll for push because the push token provided is already in use by another installation of a mobile app.
| PUSH ENROLLMENT FAILURE USER ALREADY ENROLLED      | Failed to enroll for push because the user is already enrolled for push.
| PUSH ENROLLMENT DROP EXISTING PUSH ENROLLMENT      | While enrolling for push, existing push enrollment was dropped.
| PUSH ENROLLMENT DROP EXISTING USER CLIENT KEYS     | While enrolling for push, existing key enrollment was dropped.
| PUSH ENROLLMENT UPDATE SUCCESS                     | The push enrollment was successfully updated.
| PUSH ENROLLMENT UPDATE FAILURE INVALID REQ         | Failed to update push enrollment because the request was malformed or had missing parameters.
| PUSH ENROLLMENT UPDATE INVALID PLATFORM            | While updating the push enrollment, an unsupported platform was used.
| PUSH ENROLLMENT UPDATE FAILURE EXISTING PUSH TOKEN | Failed to update the enrollment for push because the push token provided is already in use by another device.
| PUSH ENROLLMENT UPDATE FAILURE NOT ENROLLED        | Failed to update the enrollment for push because the device is not enrolled for push.

### Mobile authentication initialization events

| Event                                                        | Description
|--------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------
| MOBILE AUTH INIT FAILURE INVALID REQ                         | Failed to initialize mobile authentication because request was invalid. Verify that all required parameters are present. Verify that (the combination of) their values are valid.
| MOBILE AUTH INIT FAILURE INVALID AUTH PROPERTIES NAME        | Failed to initialize authentication because `type` did not match any identifier of the configured mobile authentication types.
| OTP AUTH INITIALIZED                                         | Otp authentication successfully initialized.
| OTP AUTH INIT FAILURE                                        | Otp authentication failed to initialize.
| OTP AUTH NOT AUTHENTICATED INVALID ACCESS TOKEN              | Otp authentication failed because access token is invalid.
| OTP AUTH NOT AUTHENTICATED INVALID OTP                       | Otp authentication failed because otp is invalid.
| PUSH AUTH INIT FAILURE NO PUSH ENROLLMENT                    | Failed to initialize push authentication because no authentication app enrolled for the user.
| PUSH AUTH INIT FAILURE MULTIPLE PUSH ENROLLMENTS             | Failed to initialize authentication because multiple authentication apps have been enrolled for the user. Deregister authentication apps or send a more specific request to choose one.
| PUSH AUTH INIT FAILURE MISSING APP ID                        | Failed to initialize authentication because the application identifier is missing in the request.
| PUSH AUTH INIT FAILURE MISSING DEVICE ID                     | Failed to initialize authentication because the device identifier is missing in the request.
| PUSH AUTH INIT FAILURE AUTH METHOD NOT AVAILABLE             | Failed to initialize authentication because the chosen authentication method is not available for the user.
| PUSH AUTH INIT FAILURE DISABLED                              | Failed to initialize authentication because push authentication disabled.
| PUSH AUTH INIT FAILURE MESSAGE TOO LONG                      | Failed to initialize authentication because message content was too long.
| PUSH AUTH INITIALIZED                                        | Successfully initialized push authentication for a user.
| PUSH AUTH RESEND FAILURE RESEND LIMIT REACHED                | Failed to resend push message because the resend limit was reached.
| PUSH AUTH RESEND FAILURE NO PUSH ENROLLMENT                  | Failed to resend push message because no authentication app enrolled for the user.
| PUSH AUTH RESEND SUCCESS                                     | Successfully resent push message to the push service. Note that this does not necessarily mean the push message arrived.
| PUSH AUTH PUSH SERVICE FAILURE                               | Failed to initialize authentication, unable to connect with Push Notification Service or push message rejected.
| PUSH AUTH PUSH SERVICE ACCEPTED NOTIFICATION                 | Push message was successfully sent to the push service.
| INVALID API VERSION BASED ON SERVER CONFIG                   | Usage of an older API is blocked based on the server configuration. A newer version of the API should be used.

### Mobile authentication fetch additional transaction data events

| Event                                                        | Description
|--------------------------------------------------------------|-----------------------------------------------------------------------------------------------
| MOBILE AUTH FETCH SECURED MESSAGE FETCHED                    | Successfully fetched additional transaction data for mobile authentication.
| MOBILE AUTH FETCH FAILED TRANSACTION ALREADY FETCHED         | Additional transaction data has already been fetched before.
| MOBILE AUTH FETCH FAILED REQUIRE OFFLINE AUTH                | Additional transaction data is not meant to be fetched.
| MOBILE AUTH FETCH FAILED INVALID TRANSACTION                 | Transaction is unknown.
| MOBILE AUTH FETCH FAILED INVALID ENDPOINT VERSION            | Endpoint to fetch additional transaction data is called for the wrong API version.
| MOBILE AUTH FETCH FAILED TRANSACTION EXPIRED                 | Transaction has expired.
| MOBILE AUTH FETCH FAILED NO PUSH ENROLLMENT                  | User has not enrolled for push authentication.
| MOBILE AUTH FETCH FAILED NO USER CLIENT KEYS                 | User has not enrolled their keys for mobile authentication.
| MOBILE AUTH FETCH FAILED INVALID CLIENT ID                   | Unknown identifier for the client.
| MOBILE AUTH FETCH FAILED MISSING PROFILE ID FOR OTP AUTH     | Missing parameter for profileId in a request for OTP.

### Mobile authentication answer events

| Event                                                         | Description
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------
| MOBILE AUTH AUTHENTICATED                                     | Successfully authenticated via mobile authentication.
| MOBILE AUTH NOT AUTHENTICATED                                 | User rejected authentication via mobile authentication.
| MOBILE AUTH NOT AUTHENTICATED INVALID TOKEN                   | Invalid mobile authentication answer received because of an invalid authentication token.
| PUSH AUTH NOT AUTHENTICATED INVALID TRANSACTION               | Invalid push authentication answer received because of an unknown transaction.
| PUSH AUTH NOT AUTHENTICATED REQUIRE OFFLINE AUTH              | Invalid push authentication answer received because transaction requires offline authentication.
| PUSH AUTH NOT AUTHENTICATED TRANSACTION EXPIRED               | Invalid push authentication answer received because the transaction has expired.
| PUSH AUTH NOT AUTHENTICATED INVALID ENDPOINT VERSION          | Invalid push authentication answer endpoint version was used for authentication transaction.
| PUSH AUTH NOT AUTHENTICATED FAILED TO DECRYPT MESSAGE         | Invalid push authentication answer received because it could not be decrypted.
| PUSH AUTH NOT AUTHENTICATED INVALID REFRESH TOKEN             | Failed to answer push message due to invalid refresh token.
| PUSH AUTH NOT AUTHENTICATED INVALID STATE                     | Invalid push authentication answer received because of invalid transaction state.
| PUSH AUTH NOT AUTHENTICATED NO PUSH ENROLLMENT                | Invalid push authentication answer received because of the lack of user push enrollment.
| PUSH AUTH NOT AUTHENTICATED NO USER CLIENT KEYS               | Invalid push authentication answer received because of the lack of user client keys.

### Push authentication transaction signing events

| Event                                                         | Description
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------
| PUSH AUTH TRANSACTION SIGNING SUCCEEDED                       | Verify transaction signature request succeeded.
| PUSH AUTH TRANSACTION SIGNING FAILED NO USER CLIENT KEYS      | Invalid verify transaction signature request because user has no client keys configured.
| PUSH AUTH TRANSACTION SIGNING FAILED INVALID TRANSACTION      | Invalid verify transaction signature request because of an unknown transaction.
| PUSH AUTH TRANSACTION SIGNING FAILED REQUIRE OFFLINE AUTH     | Invalid verify transaction signature request because transaction requires offline authentication.
| PUSH AUTH TRANSACTION SIGNING FAILED ENDPOINT NOT ALLOWED     | Endpoint not allowed for unsigned authentication transactions.
| PUSH AUTH TRANSACTION SIGNING FAILED TO DECRYPT MESSAGE       | Invalid verify transaction signature request because it could not be decrypted.
| PUSH AUTH TRANSACTION SIGNING FAILED INVALID TRANSACTION DATA | Invalid verify transaction signature request because transaction signature data contains invalid parameters.
| PUSH AUTH TRANSACTION SIGNING FAILED NOT AUTHENTICATED        | Invalid verify transaction signature request because the transaction is not authenticated.
| PUSH AUTH TRANSACTION SIGNING FAILED TRANSACTION EXPIRED      | Invalid verify transaction signature request because the transaction has expired.
| PUSH AUTH TRANSACTION SIGNING FAILED INVALID STATE            | Invalid verify transaction signature request because the transaction is in invalid state.
| PUSH AUTH TRANSACTION SIGNING FAILED INVALID SIGNATURE        | Invalid verify transaction signature request because the signature was invalid.
| PUSH AUTH TRANSACTION SIGNING FAILED ERROR EXPORTING PUBLIC KEY | Invalid verify transaction signature request because exporting of the public key failed.
| PUSH AUTH TRANSACTION SIGNING INVALID SIGNATURE OVER SIGNING METADATA | Invalid verify transaction signature request because the transaction signature was invalid.

### Push authentication audit events

| Event                                                         | Description
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------
| PUSH AUTH CALLBACK SENT                                       | The authentication initializer was informed about the authentication result via a callback.
| PUSH AUTH CALLBACK FAILED                                     | An error occurred while sending a callback to the authentication initializer.
| PUSH AUTH VALIDATION PIN LIMIT EXCEEDED                       | Exceeded allowed push with PIN answer attempts due to invalid PIN, therefore the user is revoked for push enrollment for this app installation.
| PUSH AUTH VALIDATION FINGER PRINT LIMIT EXCEEDED              | The wrong finger print was used via push authentication, therefore fingerprint authentication is revoked for this user.
| PUSH AUTH VALIDATION CUSTOM AUTHENTICATOR LIMIT EXCEEDED      | The wrong custom authenticator access token was used via push authentication, therefore the used custom authenticator is revoked for this user.
| PUSH AUTH TRANSACTION RETRIEVED                               | Authentication transaction successfully retrieved.
| PUSH AUTH TRANSACTION NOT RETRIEVED                           | Failed to retrieve authentication transaction, invalid transaction id.
| PUSH AUTH FETCH SECURED MESSAGE FETCHED                       | Successfully fetched secured authentication message.
| PUSH AUTH FETCH FAILED INVALID TRANSACTION                    | Failed to fetch secured authentication message due to invalid transaction id.
| PUSH AUTH FETCH FAILED TRANSACTION ALREADY FETCHED            | Failed to fetch secured authentication message due to transaction can be fetched only once.
| PUSH AUTH FETCH FAILED TRANSACTION EXPIRED                    | Failed to fetch secured authentication message due to expired transaction.

### SMS authentication events

| Event                                                         | Description
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------
| SMS AUTH AUTHENTICATED                                        | Successfully authenticated via SMS
| SMS AUTH FAILURE INVALID TRANSACTION                          | Failed to authenticate via SMS, invalid transaction identifier.
| SMS AUTH INIT FAILURE PHONE NUMBER INVALID OR MISSING         | Failed to initialise SMS authentication, phone number missing or invalid.
| SMS AUTH INIT FAILURE FAILED TO SEND SMS                      | Failed to initialise SMS authentication - failed to send SMS.
| SMS AUTH INIT FAILURE DISABLED                                | Failed to initialise sms authentication, functionality disabled.
| SMS AUTH INIT MESSAGE SENT                                    | Sms authentication message has been successfully sent.
| SMS AUTH NOT AUTHENTICATED INVALID VERIFICATION CODE          | Failed to authenticate via SMS, wrong verification code.
| SMS AUTH VALIDATION LIMIT EXCEEDED                            | Exceeded allowed SMS code verification attempts, transaction revoked.
| SMS RESEND SUCCEEDED                                          | Resending of sms succeeded.
| SMS RESEND FAILED                                             | Resending of sms failed. The details field contains the cause.

## Custom Authenticators audit events

| Event                                                       | Description                                                                                                                       |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| CUSTOM AUTHENTICATOR BAD REQUEST                            | Custom Authenticator API was reached, but the request was invalid. The details field contains the endpoint which was attempted.   |
| CUSTOM AUTHENTICATOR DISABLED                               | Custom Authenticator feature is disabled.                                                                                         |
| CUSTOM AUTHENTICATOR INVALID CLIENT                         | Invalid client was used to attempt Custom Authenticators API. The reason is provided in details field.                            |
| CUSTOM AUTHENTICATOR INVALID AUTHENTICATOR                  | Invalid client was used to attempt Custom Authenticators API. The reason is provided in details field.                            |
| CUSTOM AUTHENTICATOR ERROR                                  | Custom Authenticator was invoked, but an error occurred which prevented it from successfully completing.                          |
| CUSTOM AUTHENTICATOR REGISTRATION SUCCESS                   | Custom Authenticator registration request was executed with success.                                                              |
| CUSTOM AUTHENTICATOR REGISTRATION FAILURE                   | Custom Authenticator registration request was executed with failure.                                                              |
| CUSTOM AUTHENTICATOR REGISTRATION INVALID ACCESS TOKEN      | Invalid access token was provided in Custom Authenticator registration request.                                                   |
| CUSTOM AUTHENTICATOR AUTHENTICATION SUCCESS                 | Authentication using the Custom Authenticator was successful.                                                                     |
| CUSTOM AUTHENTICATOR AUTHENTICATION FAILURE                 | Authentication using the Custom Authenticator failed.                                                                             |
| CUSTOM AUTHENTICATOR DEREGISTRATION SUCCESS                 | Custom Authenticator deregistration request was executed with success.                                                            |
| CUSTOM AUTHENTICATOR DEREGISTRATION INVALID ACCESS TOKEN    | Invalid access token was provided in Custom Authenticator deregistration request.                                                 |
| CUSTOM AUTHENTICATOR RESPONSE INVALID                       | The Onegini Extension Engine returned an unknown (error) status in the response.                                                              |
| CUSTOM AUTHENTICATOR USER ALREADY REGISTERED                | Custom Authenticator is already registered for the user.                                                                          |
| CUSTOM AUTHENTICATOR USER NOT REGISTERED                    | The user is not registered for this Custom Authenticator.                                                                         |
| CUSTOM AUTHENTICATOR DOES NOT EXIST                         | The requested Custom Authenticator does not exist.                                                                                |
| CUSTOM AUTHENTICATOR UNKNOWN USER                           | The user was not recognized by the system.                                                                                        |
| CUSTOM AUTHENTICATOR INTERNAL SERVER ERROR                  | Onegini Access encountered an unrecoverable error during one of the Custom Authenticator actions.                               |

## User registration events

### Custom Registration audit events

| Event                                                       | Description                                                                                                                                        |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| CUSTOM REGISTRATION BAD REQUEST                             | Custom Registration API was reached, but the request was invalid. The details field contains the endpoint which was attempted.                     |
| CUSTOM REGISTRATION DISABLED IDENTITY PROVIDER              | The specified Identity Provider is either disabled or not configured to the requesting application.                                                |
| CUSTOM REGISTRATION INVALID IDENTITY PROVIDER               | The specified Identity Provider does not exist.                                                                                                    |
| CUSTOM REGISTRATION INVALID SCOPE                           | The scope was not correct for this ClientId.                                                                                                       |
| CUSTOM REGISTRATION SCRIPT ERROR                            | The Onegini Extension Engine encountered an error when executing the Custom API Script.                                                                    |
| CUSTOM REGISTRATION INVALID TRANSACTION                     | The transaction does not exist or is expired. Also sent when Identity Provider or ClientId are different than at the time of transaction creation. |
| CUSTOM REGISTRATION INIT SUCCESS                            | The init script for this Custom API has executed with a success status.                                                                            |
| CUSTOM REGISTRATION INIT FAILURE                            | The init script for this Custom API has executed with a failure status.                                                                            |
| CUSTOM REGISTRATION BACKCHANNEL SUCCESS                     | The complete script for this Custom API has executed with a success status.                                                                        |
| CUSTOM REGISTRATION BACKCHANNEL FAILURE                     | The complete script for this Custom API has executed with a failure status.                                                                        |
| CUSTOM REGISTRATION COMPLETE SUCCESS                        | The backchannel script for this Custom API has executed with a success status.                                                                     |
| CUSTOM REGISTRATION COMPLETE FAILURE                        | The backchannel script for this Custom API has executed with a failure status.                                                                     |
| CUSTOM REGISTRATION TRANSACTION TAMPERING                   | The transaction may have been tampered as provided identity provider do not match the one in transaction.                                          |

### SAML login and logout

| Event                                                       | Description
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------
| IDP SAML LOGIN MAPPED USER ID NOT FOUND                     | The SAML IdP did not return a user identifier in the configured attribute for the `userId`.
| IDP SAML LOGIN USER ID NOT FOUND                            | The SAML IdP did not return a user identifier in its response. [Configure an attribute mapping](../topics/general-app-config/identity-providers/identity-providers.md#attribute-mapping) if this SAML IdP never returns the user identifier in the element `Subject/NameID`.
| IDP SAML LOGIN TRANSACTION ID MISSING                       | No transaction identifier found during SAML authentication. Most likely due to a missing cookie.
| IDP SAML LOGIN TRANSACTION NOT FOUND                        | No authentication transaction found in cache for SAML authentication.
| IDP SAML LOGIN REPLAY DETECTED                              | This SAML response has been processed before. This could be a replay of the same response and it will be ignored.
| IDP SAML SINGLE LOGOUT USER NOT FOUND                       | An SLO request was received, but the corresponding access grants or tokens did not exist or were already expired.
| IDP SAML SINGLE LOGOUT SUCCESS                              | A user was logged out via a IdP or SP-initiated single logout request.
| IDP SAML SINGLE LOGOUT NOT SUCCESSFUL                       | A user was not successfully logged out via an SP-initiated single logout request.
| IDP SAML SINGLE LOGOUT INITIATED                            | A SP-initiated single logout request was initiated for a user.
| IDP SAML SINGLE LOGOUT INITIATION SKIPPED                   | A SP-initiated single logout request was not successfully initiated for a user.

## Configuration change events

### Admin API audit events

| Event                                                       | Description                                                                                                                    |
|-------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------|
| API API CLIENT ALREADY EXISTS                               | An attempt was made to create an API client via the API , but a different OAuth client with the same client_id already exists. |
| API API CLIENT ADDED                                        | An API client has been created via the API.                                                                                    |
| API API CLIENT LISTED                                       | A list of API clients was returned via the API.                                                                                |
| API API CLIENT DELETED                                      | An API client was deleted via the API.                                                                                         |
| API API CLIENT UPDATED                                      | The configuration of an API client was updated via the API.                                                                    |
| API API CLIENT NOT FOUND                                    | An attempt was made to read, update or delete an API client via the API, but no API client exists with the given client_id.    |
| API API CLIENT UPDATE BAD ID                                | The client_id in the URL does not match the client_id in the request body .                                          |

### Configuration API audit events

| Event                                                       | Description                                                                                                                           |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| API MOBILE PLATFORM VERSION CREATED                         | The new version of mobile application was added via the API.                                                                          |
| API MOBILE PLATFORM VERSION UPDATED                         | The one of mobile application's version was updated via the API.                                                                      |
| API MOBILE PLATFORM VERSION NOT FOUND                       | Unable to find the mobile application version that was requested via the API.                                                         |
| API MOBILE PLATFORM VERSION FOUND                           | A specific version of the mobile platform version was returned via the API.                                                           |
| API MOBILE PLATFORM VERSION LISTED                          | A list of mobile application's versions was returned via the API.                                                                     |
| API MOBILE PLATFORM VERSION ALREADY EXISTS                  | Unable to create a mobile application's version config via the API because of duplicated version returned.                            |
| API MOBILE PLATFORM VERSION EXPORTED                        | A mobile application configuration file was returned via the API.                                                                     |
| API MOBILE PLATFORM INVALID                                 | The platform was invalid. Only `android` and `ios` are valid platforms.                                                               |
| API MOBILE APPLICATION NOT FOUND                            | Unable to find mobile application.                                                                                                    |
| API WEB CLIENT ALREADY EXISTS                               | An attempt was made to create an Web client via the admin API , but a different OAuth client with the same client_id already exists.  |
| API WEB CLIENT ADDED                                        | A Web client has been created via the API.                                                                                            |
| API WEB CLIENT LISTED                                       | A list of Web clients was returned via the API.                                                                                       |
| API WEB CLIENT DELETED                                      | A Web client was deleted via the API.                                                                                                 |
| API WEB CLIENT UPDATED                                      | The configuration of a Web client was updated via the API.                                                                            |
| API WEB CLIENT NOT FOUND                                    | An attempt was made to read, update or delete an Web client via the API, but no Web client exists with the given client_id.           |
| API WEB CLIENT UPDATE BAD ID                                | The client_id in the URL does not match the client_id in the request body.                                                            |
| API DUM ENGINE CONFIG LISTED                                | A list of Delegated Administration Configuration was returned via the API.                                                                          |
| API DUM ENGINE CONFIG NOT FOUND                             | An attempt was made to read, update or delete an Delegated Administration Configuration via the API, but no configuration exists with the given id. |
| API DUM ENGINE CONFIG CREATED                               | A Delegated Administration Configuration has been created via the API.                                                                              |
| API DUM ENGINE CONFIG UPDATED                               | A Delegated Administration Configuration was updated via the API.                                                                                   |
| API DUM ENGINE CONFIG DELETED                               | A Delegated Administration Configuration was deleted via the API.                                                                                   |
| API DUM ENGINE CONFIG ALREADY EXISTS                        | An attempt was made to create a Delegated Administration Configuration via the admin API, but a different configuration with the same id already exists.|

## Geolocation audit events

| Event                                                       | Description                                                                                                                           |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| GEOLOCATION CHECK EXTERNAL CALL SUCCESS                     | A successful call to an external geolocation service was made.                                                                        |
| GEOLOCATION CHECK CACHE SUCCESS                             | A geolocation check was requested, but the response is already cached.                                                                |
| GEOLOCATION CHECK SKIPPED DATA MISSING                      | An attempt to execute geolocation check was skipped because the ip address was missing.                                               |
| GEOLOCATION CHECK RESPONSE PARSING FAILED                   | A call to the geolocation service was made, but either the response was malformed or did not contain all the required fields.         |
| GEOLOCATION CHECK EXTERNAL CALL FAILED                      | A call to the geolocation service could not be made.                                                                                  |
| GEOLOCATION CHECK FAILED                                    | An attempt to execute geolocation check was made, but failed for an unknown reason.                                                   |

## App To Web SSO events

| Event                                                       | Description                                                                                                                                                                      |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| APP TO WEB SSO TOKEN CREATED                                | A successful call of App To Web SSO service.                                                                                                                                     |
| APP TO WEB SSO DISABLED                                     | A call was made to use the feature, but it was disabled.                                                                                                                         |
| APP TO WEB SSO INVALID IDP TYPE                             | A call was made to use the feature, but the associated IDP was not of the Onegini Type.                                                                                          |
| APP TO WEB SSO FAILED                                       | A call was made to use the feature, but it was failed for some reason. Refer to the 'details' for more information.                                                              |
| APP TO WEB SSO APP TO WEB SSO TARGET URL NOT CONFIGURED     | A call was made to use the feature, but it failed because the `target_url` is not configured in [Consumer Identity Manager](https://docs-single-tenant.onegini.com/cim/stable/idp)|

## Store Key Value events

| Event                                                       | Description                                                                                                                                                                       |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| STORE KEY VALUE SUCCESSFUL                                  | The payload was stored successfully                                                                                                                                               |
| STORE KEY VALUE API DISABLED                                | The Storage API is disabled for the specified identity provider                                                                                                                   |
| STORE KEY VALUE INVALID ACCESS TOKEN TYPE                   | The access token provided was not of the correct type. Should use client credentials (anonymous resource calls)                                                                   |
| STORE KEY VALUE INVALID IDP TYPE                            | The identity provider that was specified was not of the Onegini type.                                                                                                             |
| STORE KEY VALUE INVALID IDP CONFIG                          | The configuration on the Onegini IDP for the Storage API is not correct (bad or missing credentials).                                                                             |
| STORE KEY VALUE FAILED                                      | An error occurred when storing the value in the keystore. Typically an issue in the [Consumer Identity Manager](https://docs-single-tenant.onegini.com/cim/stable/idp) application.|