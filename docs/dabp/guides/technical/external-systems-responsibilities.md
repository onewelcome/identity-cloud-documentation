# Responsibilities of the external system connected to DABP

## Securing resources
DABP is an external system that allows assigning policies to groups and people but does not enforce any rules regarding those policies.
This means that DABP does not provide any security mechanics for external systems.
It's the responsibility of the external system to use information about groups and policies assigned to a person to perform all the security-related actions.
From the external system perspective, DABP can be considered as a system that only stores configuration related to user privileges in the external system. 

The external system will receive the policy information in a `claim` that is provided by the Identity Provider. An example of this claim can be found in the [Person report](person-report.md#Example). 

## Keeping configuration up to date
DABP exposes an API that allows manipulation of policies, groups, and users.
External systems should use this API to keep DABP up to date with information about assignments of groups and permissions to users if some external tool is used to change that information (e.g. Salesforce)
