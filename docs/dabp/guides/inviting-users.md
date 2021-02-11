# Inviting users to DABP
To invite users to DABP you must have `Manage group members` permission.
Inviting a user is always a contextual action related to the currently viewed group.
For example in the below screen clicking "New User" would start the process of inviting a user to a group named `sub2` that is a subgroup of `test_group_for_docs`

![new-user-button](../img/new-user-button.png)

On the invite details, you must provide the email address and personal information of the person you want to invite. You can also assign him permissions and policies upfront.

## Why sometimes I see person details when inviting a new user
Inviting a user is in fact granting him access to a specific group.
DABP does not hold personal data but does keep references to person profiles to be able to link them with defined groups and policies.
So when you enter user email address DABP will check if configured identity management system has information about this person. If so it will return the details required for the invitation.
The person that is being invited can have an account in the identity management system because he is already part of another DABP group or has registered an account by other means.
