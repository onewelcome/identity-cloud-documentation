# In-depth view on managing policies in groups
A user of a group can be assigned any policy that is available in the group.
DABP allows you to create subgroups of any group. Policies in the subgroup are not restricted by the parent group.
That means that a subgroup can have more policies assigned than its parent group.

## Managing policies in the group
To add/remove a policy to a group you should select the group that you want to assign policies to on the group list screen.
After clicking on the desired group a modal window with group information will appear. Click on the vertical ellipsis button and select `Edit group`.
Now you can select which policies can be assigned to the group.
![edit group dialog](../img/edit-group-dialog.png)
After selecting the required policies click save to confirm your choice.

## How managing policies on a group level affects subgroup policies.
Policies that can be assigned to a subgroup are not restricted by the parent group. That means you can create a subgroup and assign policies to it that are not present in the parent group.
Adding or removing policies from a group does not affect the subgroup.

