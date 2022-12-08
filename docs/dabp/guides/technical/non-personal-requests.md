# Non-personal requests

By default, DABP personal endpoints require OIDC token and are secured by checking the caller's permissions. A caller is i.e. a CIM/DABP
person.

Non-personal request support enables DABP API calls unrelated to a personal user.

## Read endpoints

Endpoints that do not change the application's state are accessible with a valid JWT token containing a dedicated scope `dabp_read`.

Example endpoints:

- get group
- get group's policies/resources
- get person details

## Write scope

Endpoints that modify the application's state are accessible with a valid JWT token containing a dedicated scope `dabp_write`.

Example endpoints:

- add/remove/update subgroup
- add/remove/update policies
- add/remove/update resources
- add/remove/update group member
- assign/unassign group/member policies/resources/permissions

## Configuration

Dedicated read/write scope names are customizable via parameters:

- `dum.engine.auth.oidc-read-scope-name`
- `dum.engine.auth.oidc-write-scope-name`
