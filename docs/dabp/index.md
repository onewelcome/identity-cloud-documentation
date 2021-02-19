# Delegated Administration for Business Partners
Our Delegated Administration for Business Partners (DABP) tool lets you assign just the right amount of autonomy and responsibility to each distribution partner. This allows your partners to manage their own organizations while you keep a bird’s eye view. 

It works as follows:

1.  You (the administrator) create an account for a distribution partner (a superuser) and assign products to their account.
2.  The superuser then creates accounts for each relevant employee to give them access to your system (the users).
3.  The superuser will set up different roles with corresponding access authorizations and responsibilities, and assign them to the users.

This system allows your distribution partners to take care of their own user management, assign duties and responsibilities and keep up with internal changes (employees leaving, getting promoted, getting certified, reorganizations, etc.). This will significantly lighten the load on your B2B support center.

Delegated Administration for Business Partners is offered as an additional feature you can add to your Onegini Identity & Single Sign On module.
It can be configured easily for the needs of different industries and client-types.

## Main concepts
All of the concepts below can be easily renamed to suit your industry and client-types.

### Groups
A group has a name and is a collection of people and policies. It might represent something like an organization, an intermediary, or a business line inside a company. Group can have subgroups, to each group or subgroup you can add people which are then members of that group.
A group can also have 1 or more policies assigned to it. A policy has to be assigned to a group if you want to assign it to a person.

#### Group attributes
Sometimes it is necessary to have an additional reference to a group for usage in an internal system (e.g. Chamber of Commerce number). These references can be added to a group as an attribute. Check out [How and when to use custom attributes on groups](./guides/group-attributes.md) for more information.

### Person
A person in DABP represents a personal user account. It represents user data, its relation with groups, policies, and permissions within the system. A single person can be a member of several groups.

### Policies
A policy represents the rights/privileges a person has in the end system of the customer. They are not linked to user permissions inside DABP.
Policies are exposed outside of DABP to customer end systems. Check out [In-depth view on policies](./guides/policies-in-depth.md) for details.

### Permission
A permission represents the rights/privileges a person has in the DABP tool and can be assigned to a person.
Permissions are strictly linked to the DABP system and are not exposed outside of DABP and have no meaning for external systems. Check out [In-depth view on permissions](./guides/permissions-in-depth.md) for details.


## Topic Guides
### Concepts in depth
- [In-depth view on permissions](./guides/permissions-in-depth.md)
- [In-depth view on policies](./guides/policies-in-depth.md)

### Managing groups and users
- [Inviting users in DABP](./guides/inviting-users.md)
- [Managing subgroups best practices](./guides/subgroups-best-practices.md)
- [How and when to use custom attributes on groups](./guides/group-attributes.md)

### Administration
- [How to create a new policy via the API](./guides/create-policy-via-api.md)

### Technical details
- [Responsibilities of external system connected to DABP](./guides/external-systems-responsibilities.md)
- [Person report - What is it and how to generate it](./guides/person-report.md)
