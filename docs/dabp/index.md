# Delegated Administration for Business Partners
Delegated administration for business partners (DABP) is an add-on feature for your platform.
It is a ready-to-use feature that enables you to let selected (third-party) contact person manage access and authorizations for other users in a secure way.
This helps you reduce the management overhead. It can be configured easily for the needs of different industries and client-types.

## Main concepts

### Groups
A group is generic term. It gathers people and policies. You can think about it as employer, intermediary or a business line inside a company.
A group can have subgroups, to each group or subgroup you can add people which are then members of that group.
A group can also have 1 or more policies added to it. A policy has to be added to a group if you want to assign it to a person.

### Person
A person in DABP should be considered as a personal user account. It represents user data, his relation with groups, policies and permissions within the system.

### Permission
Permissions can be assigned to people. Permissions are strictly linked to the DABP system and control what users can do in DABP.
They are not exposed outside of DABP and have no meaning for external systems.

### Policies
A policy represents the rights/privileges a person has in the end system of the customer. They are not linked to user permissions inside DABP.
Policies are exposed outside of DABP to customer end systems.

### Group custom attributes
Sometimes a customer would like to add an attribute to a group, for example, a reference to a Salesfore group which allows synchronization between Salesforce and DABP.
These attributes can be added to a group with custom attributes. 

## Topic Guides
- [How to create a new policy via the API](./guides/create-policy-via-api.md)
- [Person report - What is it and how to fetch it](./guides/person-report.md)
- [In-depth view on managing policies in groups](./guides/managing-policies.md)
- [Responsibilities of external system connected to DABP](./guides/external-systems-responsibilities.md)
- [In-depth view on permissions](./guides/permissions-in-depth.md)
- [In-depth view on policies](./guides/policies-in-depth.md)
- [Inviting users in DABP](./guides/inviting-users.md)
- [Managing subgroups best practices](./guides/subgroups-best-practices.md)
- [How and when to use custom attributes on groups](./guides/group-attributes.md)
