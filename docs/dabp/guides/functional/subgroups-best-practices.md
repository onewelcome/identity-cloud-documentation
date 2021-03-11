# Best practices for managing subgroups
Each group can have several subgroups.
Since subgroups have some additional restrictions comparing to standard groups you should always think about the use case.
Sometimes, using a subgroup can be very beneficial, sometimes using a standard group is a better option.
This topic guide will explain the differences between groups and subgroups and will try to explain when to use subgroups.

## Differences between group and subgroup
Groups are independent of each other.
This might be a good fit when the groups represent physical or legally separated entities.    
Subgroups are dependent on their parent groups (ancestors). Subgroups can be configured to access only some of the policies of the parent.
This makes subgroups a good fit when you want to represent a line of business in a legal entity (ie. separate life insurances from car insurances)

### Permissions
Assigning permission to a group member means that he will also have this permission in all subgroups.
Subgroups, therefore, make it possible to have 'superusers' that manage multiple groups.

If a person has permissions for a particular group, this does not impact the permissions of other groups on the same level.
This makes it possible to have different persons manage different groups.
The same person might be able to manage multiple groups on the same level, however, this requires that person to get assigned the permissions manually to each group.

## Policies on Group level
A subgroup cannot be connected with a policy that is not linked to its parent group.
Assigning a policy to a group does not automatically assign it to its subgroups.
This setup makes it possible to limit which policies can be assigned to members of a particular subgroup.