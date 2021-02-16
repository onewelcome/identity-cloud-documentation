# Managing subgroups best practices
Groups can either get created at the same or as a subgroup of each other.
The different options got different advantages that impact how you manage your groups.

Groups on the same level are independent of each other. 
This might be a good fit when the groups represent physical or legally separated entities.    
Subgroups got dependencies on their parent groups (ancestors). 
This makes it possible to orchestrates permissions and policies together in an easier way.      

## Permissions
Assigned permission is also giving the member permissions to all the subgroups.

Sub-groups thus make it possible to have 'super users' that manage multiple groups.
He will automatically manage subgroups that are created in the future.

Assigned permission is not impacting the permissions of other groups on the same level.
This makes it possible to have different users manage different groups. 
A person can still manage groups on the same level. 
However, this requires that person to get assigned the permissions manually to each group.  


## Policies
A subgroup can only get assigned policies that are assigned to its parent group.
This makes it possible to limit what policies a group member can assign to a subgroup.

Two groups on the same level can get assigned the same policies as they have the same parent group.
