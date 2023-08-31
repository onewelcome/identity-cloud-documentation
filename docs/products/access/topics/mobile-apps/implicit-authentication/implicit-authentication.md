# Implicit authentication

Implicit authentication can be used to authenticate users without explicitly interaction with a user. With this feature enabled, users can
be authenticated implicitly at any time if they have previously registered with their device. This feature can be useful for fetching (
personal) data that is not sensitive for users on that device. An example is to fetch the first name to show a greeting when the user opens
their app. When more
sensitive data is needed, the user must use a different authentication method, like the pin or biometric authentication.

A resource gateway can validate an implicit access token using [token introspection](../../../api-reference/token-introspection.md).

## Enable implicit authentication

To enable implicit authentication for your app, first head over to `Configuration` &rightarrow; `Applications` in the OneWelcome Access admin.
Once there you can create a new application or editing one existing. Either way, to enable implicit authentication, follow these steps.

- Under OAuth settings, enable the 'User Registration' flow.
- Under User authentication, enable 'Implicit authentication'.

The User Registration flow must be enabled so users can register before using implicit authentication.
