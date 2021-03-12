# Person Report
The Person Report is a record describing all groups a person is a member of.
This includes all policies he has been assigned to in those groups.

The report can be fetched via an API call.
The details can also be returned as JWT claim (OIDC) and as SAML attribute.

## Person Report and Permissions
While a person report can be used by applications to check a person's rights/privileges or answer authorization questions, it does not include [DABP permissions](../concepts/permissions-in-depth.md).
Permissions are DABP specific and should not be used by external applications.

However, a special policy, `role_superuser`, will be added to report on a group level if the person has any permission within the group.

## Get Person Report via API
In this guide, we will assume the Onegini Identity Cloud is available under `https://dabp.onegini.com` url.
The API requires authentication using OIDC. Make sure you add a valid access token to the request header.

The person report for a person with a given identifier (`referenceId`) can be received by
doing a GET request to `https://dabp.onegini.com/delegate/api/v2/people/{referenceId}/report`.

Person report is meant to be used only by external systems and not directly by logged-in users.
To achieve this, the external system should be enrolled in the access component as a client and should request a dedicated scope `dabp_person_report` when obtaining the token.
This token should be then provided when requesting the person report.

There are two scenarios when the report is requested:

* When the user is logged in to the external system and it needs to check users policies (so linked with real user activity)
* When the external system does some synchronization work and needs to check users information (so not linked with real user activity)

To support both scenarios and keep the information about user activity, generation of the report will update person last activity date by default.
If you want to use the report for the second scenario (not related to user activity) please execute the request with additional parameter `skipUpdatingActivity=true`
ie `https://dabp.onegini.com/delegate/api/v2/people/{referenceId}/report?skipUpdatingActivity=true`


### Example

Example request call
```
curl --location --request GET 'https://dabp.onegini.com/delegate/api/v2/people/eb82d9a9-bba5-4bbb-a9fc-508ce6f8705c/report' \
--header 'Content: application/json' \
--header 'Authorization: Bearer  <access token>' \
--header 'Content-Type: application/json'
```

Example response from the server
```
{
 "groups": [
    {
      "id": "2374b2db-e690-4f3a-89e0-ccd5aaf6c601",
      "name": "Intermediary A",
      "policies": [
        {
          "id": "77195a4e-c610-4716-b790-721a5fdde1e6", 
          "name": "Pension"
        },
        {
          "id": "6fbe9789-ee84-46eb-9234-2d2d711a0328", 
          "name": "Insurance"
        }
      ],
      "attributes": [
        {
          "name": "salesforceId",
          "value": "12345"
        }
      ]
    },
    {
      "id": "2374b2db-e690-4f3a-89e0-ccd5aaf6c601",
      "name": "Intermediary B",
      "policies": [
        {
          "name:" "role_superuser"
        },
        {
          "id": "77195a4e-c610-4716-b790-721a5fdde1e6",
          "name": "Pension"
        },
        {
          "id": "6fbe9789-ee84-46eb-9234-2d2d711a0328",
          "name": "Insurance"
        }
      ],
      "attributes": []
    }
  ]
}
```

In the example response, we can see that group `Intermediary A` has an additional attribute called `salesforceId`.
Also in group `Intermediary B` user has at least one DABP privilege assigned as an additional policy `role_superuser` is added.
Please note that `role_superuser` is an artificial policy, so it does not have an ID and cannot be accessed by API or UI.
