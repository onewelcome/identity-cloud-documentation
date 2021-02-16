# In-depth view on policies
A policy represents the rights/privileges a person has outside DABP. They are not linked to user permissions inside DABP.
Policies are available outside of DABP via the API.
Policies are assigned to a person for each group he is a member of.
DABP does not use policies to determine if a user is eligible to do something when they have a policy assigned, this is the responsibility of the external system.
For example, you can create a policy SELL_LIFE_INSURANCE and assign it to a group member. 
This could tell the end system to allow this person to sell life insurances.

## Managing policies for a group member
To manage user policies open the user details modal, click the vertical ellipsis and choose "Change membership".
This will switch the modal to the edit mode where you will be able to change the user policies.
Please note that you can only assign policies that are linked to the group in which you are editing a person.

![edit person dialog](../img/edit-person.png)

Click save to confirm your choices.

If you have the privilege to manage person policies you **can assign policies that are not assigned to you**.

## Relation between groups, members and policies
Both groups and members can have policies assigned. The difference is that people only can have assigned a policy that is already assigned to a group.
This means that assigning policies to a person is a contextual operation.
Let's assume we have 2 policies `Sell life insurance` and `Sell car insurance`. We have 2 groups `Life` and `All products`.
A user `User A` is a member of both groups. Policy `Sell life insurance` is linked to group `Life` and `All products`.
Policy `Sell car insurance` is linked only to group `All products`
When you will view the details of `User A` you will see that he is a member of multiple groups.
The "Change membership" button is available for each group. You will be able to add the `Sell life insurance` policy to the user in both groups, but `Sell car insurance` only for the `All products` group.
Linking a user with a policy for one group does not mean he has access to the same policy in other groups linked to that policy.
It's perfectly fine to link the user to `Sell life insurance` in group `Life` and do not link him with this policy in another group.
It is the responsibility of the external system to verify user policies inside a particular group.
