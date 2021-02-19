# In-depth view on policies
A policy represents the rights/privileges a person has outside DABP. They are not linked to user permissions inside DABP.
Policies are available outside of DABP via the API.
Policies are assigned to a person for each group he is a member of.
DABP does not use policies to determine if a user is eligible to do something when they have a policy assigned, this is the responsibility of the external system.
For example, you can create a policy `SELL_LIFE_INSURANCE` and assign it to a group member. 
This could tell the end system to allow this person to sell life insurances.

## Relation between groups, persons, and policies
Both groups and persons can have policies assigned to it. The difference is that a person can only have policies assigned that are also assigned to a group.
This means that assigning policies to a person is a contextual operation.
Let's take this scenario:
* There are 2 policies `Sell life insurance` and `Sell car insurance`. 
* There are  2 groups `Life` and `All products`.
* Policy `Sell life insurance` is linked to group `Life` and `All products`.
* Policy `Sell car insurance` is linked only to group `All products`
* A user `John Doe` is a member of both groups. 


| Groups                | `Sell life insurance` | `Sell car insurance` |
| :-------------------- | :-------------------: | :------------------: |
| **All products**      |      ✅       |    ✅     |
| **Life** 				|      ✅       |    ❌     |


When you will view the details of `John Doe` you will see that he is a member of multiple groups.
The "Change membership" button is available for each group. You will be able to add the `Sell life insurance` policy to the user for both groups, but `Sell car insurance` only for the `All products` group.

Linking a user with a policy for one group does not mean he has access to the same policy in other groups linked to that policy.
It's perfectly fine to link the user to `Sell life insurance` in group `Life` and do not link him with this policy in another group.
It is the responsibility of the external system to verify if a user has the required policies for the required group.

## The hierarchal relationship between groups, subgroups, and policies
It is not possible to assign more policies to a subgroup than its parent. This means that the DABP tool enforces that a superuser can only assign policies to groups if the parent also has that policy. 

### Adding policies to a group
Adding a policy to a group does **not** assign this policy to all subgroups, it only makes it possible to assign this policy also to the subgroups. 
Also, Adding a policy to a group does **not** assign it to all the group members, it only makes it possible to assign the policy to the group members.

### Removing 
Removing a policy from a parent will result in the removal of the policy for all subgroups and members because the allowed policies are restricted by the policies assigned to the parent group.

## Assigning or removing policies to a group
To add/remove a policy to a group you should select the group that you want to assign policies to on the group list screen.
After clicking on the desired group a modal window with group information will appear. Click on the vertical ellipsis button and select `Edit group`.
Now you can select which policies can be assigned to the group.

![edit group dialog](../img/edit-group-dialog.png)

After selecting the required policies click save to confirm your choice.

## Assigning or removing policies to a person
To manage users' policies open the user details modal, click the vertical ellipsis and choose "Change membership".
This will switch the modal to the edit mode where you will be able to change the user policies.
Please note that you can only assign policies that are linked to the group in which you are editing a person.

![edit person dialog](../img/edit-person.png)

Click save to confirm your choices.

If you have the permission to `manage person policies` you **can assign policies that are not assigned to you**.