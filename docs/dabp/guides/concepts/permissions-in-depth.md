# In-depth view on permissions
A permission represents the rights/privileges a person has in the DABP tool and can be assigned to a person.
Permissions are strictly linked to the DABP system and are not exposed outside of DABP and have no meaning for external systems. 

In total are there 6 permissions:

- **Invite/remove group members** enables the user to add, edit, and delete members of the group, and its subgroups.
- **Assign permissions to group members** enables the user to add, edit, and delete permissions of group members.
- **Assign policies to group members** enables the user to add, edit, and delete policies of group members.
- **Add/edit/delete groups** enables the user to add, edit, and delete subgroups
- **Assign policies to groups** enables the user to add, edit, and delete policies on a group level.
- **Add/edit/delete policies** enables the user to add, edit, and delete policies to the system.

If a person has any permissions for any (sub)group we refer to that user as a superuser.
If a person has all permissions on the root level we refer to that user as an administrator.

## Permission inheritance
Permissions are inherited within groups. This means that if a user was invited to DABP to join `group A` with permissions to `Invite/remove group members`, and `Add/edit/delete groups`, then they will have those permissions for every subgroup of `group A`.

## Managing permissions of other users
A user that has the `Invite/remove group members` permission can add and remove users from that group.
A user that has the `Assign permissions to group members` permission can assign permissions to a person.
When inviting a person you can select what permissions he will have in the group. 

The most important rule is that you **cannot grant permissions that you don't have yourself**

For example:  

- User A has all permissions  
- User B has permissions `Invite/remove group members` and `Assign permissions to group members`  
- They both are members of the same group.  
- User B cannot remove `Manage group`, `Manage person policies`, `Manage group policies`, or `Manage policies` permission from User A.  
- User B can remove `Manage group members` and `Manage permissions` from User A.  
- User B can invite a new user to their group, but can only assign `Manage group members` and `Manage permissions` or a subset when inviting him.  
- User A can invite a new user to their group and can assign all permissions or a subset.

To manage user permissions open the user details modal, click the vertical ellipsis and choose "Change membership".
This will switch the modal to the edit mode where you will be able to change user permissions.

![edit person dialog](../../img/edit-person.png)

Click save to confirm your choices.
