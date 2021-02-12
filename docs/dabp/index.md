# Delegated Administration for Business Partners
Delegated administration for business partners (DABP) is an add-on feature for your platform.
It is a ready-to-use feature that enables you to let selected (third-party) contacts manage access and authorizations for other users in a secure way.
This helps you reduce the management overhead. It can be configured easily for the needs of different industries and client-types.

## Main concepts

### Groups
A group is a generic term for what a company can be for our customers, sometimes this is called an employer or intermediary.
A group can have subgroups, to each group or subgroup you can add people which are then members of that group.
A group can also have 1 or more policies added to it. A policy has to be added to a group if you want to assign it to a person.

### Person
A person in DABP is a reference to a user of an external system, for example, CIM.
Because DABP is not the owner of the user data we store as it only stores person reference for accounts that come from external systems.

### Permission
Permissions can be assigned to people. Permissions are strictly linked to the DABP system and control what user can do in DABP.
They are not exposed outside of DABP and have no meaning for external systems.

### Policies
A policy represents the rights/privileges a person has in the end system of the customer. They are not linked to user permissions inside DABP.
Policies are exposed outside of DABP to customer end systems.

### Group custom attributes
Sometimes a customer would like to add an attribute to a group, for example, a reference to a Salesfore group which allows synchronization between Salesforce and DABP.
These attributes can be added to a group with custom attributes. 

## Topic Guides
- [How to create a new policy via the API](./guides/create-policy-via-api.md)
- [Person report - What is it and how to generate it](./guides/person-report.md)
- [In-depth view on managing policies in groups](./guides/managing-policies.md)
- [Responsibilities of external system connected to DABP](./guides/external-systems-responsibilities.md)
- [In-depth view on permissions](./guides/permissions-in-depth.md)
- [In-depth view on policies](./guides/policies-in-depth.md)
- [Inviting users in DABP](./guides/inviting-users.md)
- [Managing subgroups best practices](./guides/subgroups-best-practices.md)
- [How and when to use custom attributes on groups](./guides/custom-attributes.md)
