# In-depth view on resources
A resource represents the fine-grained access or privileges a user has outside Delegated Administration. 
For example, the resource can represent a portfolio for a specific reseller.
It can represent a portfolio of, for instance, mortgages for a specific reseller when a resource is linked to a policy for mortgages.
Each resource has a specific resource type.
Resource types define if resources of that type require a policy, and what kind of privileges user may have on the resource.
Resource type does not need to be configured with policy. If there is no policy on a resource type, then resource can be assigned to groups and members regardless of the policies configured in that group.
There can be multiple resources of the same type.
Resource types privileges represent what kind of access a user should have on the resource when they're linked together. 
For example a resource type can specify 3 privileges: `read`, `create`, `update`. When a resource of that type is assigned to user, he will get one of those privileges.
DABP does not use this information internally, it is the responsibility of the external system to verify that user has correct privileges on a resource when granting access to resource.

## Relation between groups, persons, policies and resources
Both groups and persons can have resources assigned to it. The difference is that a person can only have resources assigned that are also assigned to a group.
This means that assigning resources to a person is a contextual operation.
Let's take this scenario:

- There is resource type `Insurance` with list of privileges `read`, `write`. This type is linked with `Sell insurance` policy
- There is resource type `Mortgage` with list of privileges `sell`, `extend`. This type is linked with `Sell mortgage` policy
- There is resource type `Unrestricted` with list of privileges `read`, `write`. This type is not linked any policy
- There is a resource `Life insurance portfolio` of type `Insurance`. 
- There is a resource `Mortgage portfolio` of type `Mortgage`. 
- There is a resource `Client contact infos` of type `Unrestricted`. 
- There are 3 groups `Organization Life`, `Organization Mortgage` and `Cooperation`.
- Policy `Sell insurance` linked to the `Organization Life` and `Cooperation` groups. 
- Policy `Sell mortgage` linked to the `Organization Mortgage` and `Cooperation` groups. 
- Resource `Life insurance portfolio` is linked to group `Organization Life` and `Cooperation`.
- Resource `Mortgage portfolio` is linked to group `Organization Mortgage` and `Cooperation`.
- Resource `Client contact infos` is linked only to group `Cooperation`.
- A user `John Doe` is a member of all groups. 


| Groups                | `Life insurance portfolio` | `Mortgage portfolio` | `Client contact infos` |
| :-------------------- | :-------------------:  |:------------------: |:------------------: |
| **Cooperation**      |      ✅       |     ✅     | ✅     |
| **Organization Life** 	|      ✅       |     ❌     | ✅     |
| **Organization Mortgage** |      ❌      |      ✅     | ✅     |

`Life insurace portfolio` cannot be added to `Organization Mortgage` group because `Sell insurance` policy is not linked to `Organization Mortgage` group.
`Mortgage portfolio` cannot be added to `Organization Life` group because `Sell mortgage` policy is not linked to `Organization Life` group.
`Client contact infos` can be added to any group as it has no policy restrictions.
John Doe can have access to `Life insurance portfolio` in `Cooperation` and `Organization Life` if he has `Sell insurance` policy in those groups.
John Doe can have access to `Mortgage portfolio` in `Cooperation` and `Organization Mortgage` if he has `Sell mortgage` policy in those groups.
John Doe can have access to `Client contact infos` in all the groups regardless of the policies he has.

## The hierarchical relationship between groups, subgroups, and resources
It is not possible to assign more resources to a subgroup than its parent. This means that the DABP tool enforces that a superuser can only assign resources to groups if the parent also has that resource. 

### Adding resource to a group
Adding a resource to a group does **not** assign this resource to all subgroups, it only makes it possible to assign this resource also to the subgroups. 
Also, Adding a resource to a group does **not** assign it to all the group members, it only makes it possible to assign the resource to the group members.

### Removing 
Removing a resource from a parent will result in the removal of the resource for all subgroups and members because the allowed resources are restricted by the resources assigned to the parent group.

## Assigning or removing resources to a group
To add/remove a resource to a group you should select the group that you want to assign resource to on the group list screen.
After clicking on the desired group a modal window with group information will appear. Click on the vertical ellipsis button and select `Edit group`.
Now you can select which resources can be assigned to the group.

![edit group dialog](../../img/edit-group-dialog.png)

After selecting the required resources click save to confirm your choice.

## Assigning or removing resources to a person
To manage users' resources open the user details modal, click the vertical ellipsis and choose "Change membership".
This will switch the modal to the edit mode where you will be able to change the user resources.
Please note that you can only assign resources that are linked to the group in which you are editing a person.

![edit person dialog](../../img/edit-person.png)

When you click it, you can assign resources to member in the group. Assigning resources requires to select a privilege. By default, a `No access` privilege is selected.
Linking a user with a resource for one group does not mean he has access to the same resources in other groups linked to that resource.
It's perfectly fine to link the user to `Life insurance portfolio` in group `Life products` and do not link him with this resource in another group.
It is the responsibility of the external system to verify if a user has the required resources for the required group.
Click save to confirm your choices.

If you have the permission to `Assign resources to group members` you **can assign resources that are not assigned to you**.
