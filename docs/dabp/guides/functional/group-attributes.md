# How and when to use group attributes
When DABP is used in coordination with an external system to manage users' policies it might be beneficial to be able to add a reference to a group. This can be achieved by group attributes.

## When to use attributes?
Attributes can be helpful in a wide range of scenarios:

1. Synchronization of group information with external systems (e.g. Salesforce) - the external system identifier can be stored as an attribute to allow mapping of the group with external data
2. Storing additional information about a group:
    * when a group represents physical offices it can be addresses or contact information
    * any kind of metadata related to the group that is needed for an external system or users of DABP (e.g. Chamber of Commerce number)
3. The API exposes an endpoint that allows you to search for a group with a specified attribute

## Adding attributes to group
Attributes can be added to the group from the Group Details dialog.
Use `+ Add custom attribute` button to add a new attribute. You will need to specify a key (name) of the attribute and its value.

![edit group attributes](../../img/group-attributes.png)

Click save to confirm your changes.
