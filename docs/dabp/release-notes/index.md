# Release notes

This section contains release notes for OneWelcome DABP.

In the release notes we mention new features and bug fixes. If anything is unclear, feel free to contact [OneWelcome Support](https://support.onewelcome.com).

## Release date: 2023-11-06

### Bugs
* We have corrected the content-type used in the invitation request sent to the Customer Identity Management (CIM) system.

## Release date: 2023-08-02

### Bugs
* We fixed a bug where users without permission got stuck on a loading page. Now they get an error. 

## Release date: 2023-07-26

### Bugs
* The deprecated `/delegation/api/v1/persons/{referenceId}/report` and `/delegation/api/v1/persons/{referenceId}/report-omit-identity` endpoints now also accepts token authentication (required scope `dabp_person_report`). 

## Release date: 2023-07-10

### Improvements
* We added two new message keys to show a description of policies and permissions on the "Add user" screens. 

### Bugs
* We fixed an issue where emails were not sent when the inviter had no first name or last name registered in the IDP.

## Release date: 2023-06-27

### Bugs
* We fixed the validation of M2M tokens used to access the APIs.  

## Release date: 2023-06-20

### Improvements
* We added a CSP header to prevent the app from being loaded in an iframe.
* We added an HSTS header.

## Release date: 2023-06-16

### Improvements
* Group policies and resources are now also presented to users without "manage group permissions".

## Release date: 2023-06-13

### Bugs
* We no longer delete the cached names of a person when the person was updated without a name change. 

## Release date: 2023-06-09

### Bugs
* We no longer send the `AuthorizationMemberResourceAssignmentsChangedEvent` if resources are not changed.

## Release date: 2023-06-01

### Improvements
* We improved the performance of the application by caching more data.

## Release date: 2023-05-17

### Improvements
* We improved the performance of the `superusers` endpoint for setups with a lot of super users.

### Bugs
* We resolved a bug where an authentication dialog was shown during the logout flow.

## Release date: 2023-04-12

### Improvements
* We no longer send two emails ("invite" and "user added to group") when a new user is added.
* Added an error message to explain what goes wrong when added a user with invalid characters in the name.

## Release date: 2023-03-31

### Features
* We introduced a new feature to indicate which users are a superuser within a group. This feature can be enabled via a feature flag in the settings menu.
* Searching for a user on an email address is now possible. For searching email addresses, we use the prefix search algorithm, which allows you to search for the beginning portion of the email address.

### Bugs
* We fixed a bug where a user's status was not visible on the first loading of the application. 

## Release date: 2023-02-16

### Features
* We now redirect an end-user to the URL they requested after they authenticated. 
* It is now possible to specify which `idp` should be used for authentication using a URL param.

### Improvements
* Next to the Group name, the group id is also available in the email template that is sent when a user is added to a group. 
* When using an external IDP (e.g. AzureAD) for employees, the `dabp_user_manager` now also has the *Assign permissions to group members* permission.

## Release date: 2023-01-26

### Features
* We extended the [Person Report](../guides/technical/person-report.md) to also include the `ResourceTypes` for `resources`.

## Release date: 2022-12-20

### Features
* The API to manage the group-resource relationship now also supports the PATCH method, to make it easier to add a resource to a group.

## Release date: 2022-11-18

### Features
* We added a feature to search for groups based on a custom attribute.

## Release date: 2022-05-18

### Features
* We extended the [Person Report](../guides/technical/person-report.md) to also include policies on subgroups. This way, consuming applications know which user has access to which groups and no longer need to be aware of the hierarchy.

### Improvements
* It's now possible to manually overwrite the county code for mobile phone numbers. This allows inviting users outside of Europe. 
* We improved our internal logging regarding status syncs between our IdP and DABP.

## Release date: 2021-05-28

### Features
* Extending the Delegation-API with endpoints for managing policies on members
* Extending the Delegation-API with endpoints for managing permissions on groups and members
* Extending the Delegation-API with endpoints for deleting a person

## Release date: 2021-05-26

### Features
* The browser title is now customizable

### Bugs
* Fixed an issue with internal communications with other OneWelcome services

## Release date: 2021-04-28

### Improvements
* Removing a users last membership will now not remove users identity in CIM
* Fixed failed login with external IdP (such as AzureAD)
* Align error message for validation when phone number is rejected in CIM

## Release date: 2021-04-14

### Features
* After removing a user from a group, if he is no longer of any group his account will be removed from CIM

### Improvements
* Translation fixes and improvements
* Minor UI improvements

   - Color of the Profile icon is now configurable
   - Logo format has changed from .png to .svg
   - A preview is shown when the user details are loaded
   -  User status and app loading messages are now translatable
   -  Forms don't close anymore after inactivity
   -  Error message on "Add user" page will clear after from is closed
   -  Attributes are sorted by name
   -  More resilience against error 504
