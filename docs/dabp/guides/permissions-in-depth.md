# In-depth view on permissions
Permissions can be assigned to people. Permissions are strictly linked to the DABP system and control what users can do in DABP.
They are not exposed outside of DABP and have no meaning for external systems.
For example, "Manage group" will permit a person to manage groups, so add groups, edit groups, add group members, etc.

In total there are 6 permissions:

* **Manage group** enables the user to add, update and delete groups and subgroups
* **Manage group members** enables the user to add and delete members to groups.
* **Manage person policies** enables the user to add, update and delete policies on a personal level.
* **Manage group policies** enables the user to add, update and delete policies on a group level.
* **Manage policies** enables the user to add and delete policies to the system.
* **Manage permissions** enables the user to add, update and delete permissions on a personal level.

## Permission inheritance
Permissions are inherited within the groups. If a user was invited to the DABP system to join `group A` with permissions to `Manage group` and `Manage group members`,
then he will have those permissions in every subgroup of `group A`

## Managing permissions of other users
A user that has `Manage group members` can invite other users to the group or discard their memberships from the group.
When inviting a user you can select what permissions he will have in the group. The most important thing is that you **cannot grant permissions that you don't have yourself**
Even when you have a `Manage permissions` permission, it will only allow you to add/remove permissions from other users that you have yourself.
Example
User A has all permissions
User B has permissions `Manage group members` and `Manage permissions`
They both are in the same group.
User B cannot remove `Manage group`, `Manage person policies`, `Manage group policies`, or `Manage policies` permission from User A.
User B can remove `Manage group members` and `Manage permissions` from User A.
User B can invite a new user to their group, but can only assign `Manage group members` and `Manage permissions` when inviting him.
User A can invite a new user to their group and can assign all permissions when inviting him.

To manage user permissions open the user details modal, click the vertical ellipsis and choose "Change membership".
This will switch the modal to the edit mode where you will be able to change user permissions
![edit person dialog](../img/edit-person.png)
Click save to confirm your choices.
