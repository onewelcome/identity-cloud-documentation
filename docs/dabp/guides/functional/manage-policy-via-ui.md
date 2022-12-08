# Manage policies

You need to create a policy before you can assign it to groups and members.
Policies can be created on the "Policies" page or via the [Create Policy API](../technical/create-policy-via-api.md).

To manage policies, you must be an user with the `Add/edit/delete policies` permission on the root group.
That is the top-level group.
The "Policies" page is available under "Settings" when you have this permission.

![Policies page](../../img/policies-page.png)

## Create policies

*For technical documentation, see [How to create a new policy via the API](../technical/create-policy-via-api.md).*

You can create a new policy using the "New Policy" button on the page. You have to give the policy a unique name.
A new policy is automatically added to the root group.
Thus a superuser with the permission `Assign policies to group members` on the root level can assign it to other groups.

## Edit policies

You can edit policies by clicking on policy on the policy overview.
You can then click on the vertical ellipsis button and select "Edit policy".

!!! note

    By editing a policy, users with that policy assigned might lose access to your applications when you change the policy. This only happens when you rely on the name and not the ID of a policy.

## Delete policies

You can delete policies by clicking on the delete icon in the list of policies.
Alternatively, you can delete policies by selecting the policy and choose "Delete policy" under the vertical ellipsis menu.

!!! warning

    The policy will be removed from all groups and members when a policy is deleted.
    This might cause the users to lose access to your applications. 
