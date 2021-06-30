# In-depth view on resources
A resource represents the fine-grained access or privileges a user has outside Delegated Administration. 
For example, the resource can represent a portfolio for a specific reseller.
It can represent a portfolio of, for instance, mortgages for a specific reseller when a resource is linked to a policy for mortgages.
Each resource has a specific type which describes if resource is linked with a policy, and what kind of permissions user may have on the resource.

## Relation between groups, persons, policies and resources
Both groups and persons can have resources assigned to it. The difference is that a person can only have resources assigned that are also assigned to a group.
This means that assigning resources to a person is a contextual operation.
Let's take this scenario:

- There are 2 resources `Life insurance portfolio` and `Car insurance portfolio`. 
- There are  2 groups `Life` and `All products`.
- There is a policy `Sell life insurance` linked to the `Life` and `All products` groups. 
- Resource `Life insurance portfolio` is linked to group `Life` and `All products`.
- Resource `Life insurance portfolio` is linked to policy `Sell life insurance`.
- Resource `Car insurance portfolio` is linked only to group `All products`
- Resource `Car insurance portfolio` is not linked to any policy.
- A user `John Doe` is a member of both groups. 


| Groups                | `Life insurance portfolio` | `Car insurance portfolio` |
| :-------------------- | :-------------------: | :------------------: |
| **All products**      |      ✅       |    ✅     |
| **Life** 				|      ✅       |    ❌     |


When you will view the details of `John Doe` you will see that he is a member of multiple groups.
The "Change membership" button is available for each group. 
You will be able to add the `Life insurance porfolio` resource to the user for both groups if the user has the `Sell life insurance` policy.
You will be able to add the `Car insurance portfolio` only for the `All products` group no matter of the policies attached to the user.

Linking a user with a resource for one group does not mean he has access to the same resources in other groups linked to that resource.
It's perfectly fine to link the user to `Life insurance portfolio` in group `Life` and do not link him with this resource in another group.
It is the responsibility of the external system to verify if a user has the required resources for the required group.

## The hierarchal relationship between groups, subgroups, and resources
It is not possible to assign more resources to a subgroup than its parent. This means that the DABP tool enforces that a superuser can only assign resources to groups if the parent also has that resource. 

### Adding resource to a group
Adding a resource to a group does **not** assign this resource to all subgroups, it only makes it possible to assign this resource also to the subgroups. 
Also, Adding a resource to a group does **not** assign it to all the group members, it only makes it possible to assign the resource to the group members.

### Removing 
Removing a resource from a parent will result in the removal of the resource for all subgroups and members because the allowed resources are restricted by the resources assigned to the parent group.

## Assigning or removing policies to a group
To add/remove a resource to a group you should select the group that you want to assign resource to on the group list screen.
After clicking on the desired group a modal window with group information will appear. Click on the vertical ellipsis button and select `Edit group`.
Now you can select which resources can be assigned to the group.

![edit group dialog](../../img/edit-group-dialog.png)

After selecting the required resources click save to confirm your choice.

## Assigning or removing policies to a person
To manage users' resources open the user details modal, click the vertical ellipsis and choose "Change membership".
This will switch the modal to the edit mode where you will be able to change the user resources.
Please note that you can only assign resources that are linked to the group in which you are editing a person.


//TODO update screenshot
![edit person dialog](../../img/edit-person.png)

Click save to confirm your choices.

If you have the permission to `Assign resources to group members` you **can assign resources that are not assigned to you**.
