# Person Report - What is it and how to fetch it
The Person Report is a record describing all groups a person is a member of. 
This includes all policies he possesses for the groups.

The report allows applications to check a person's rights/privileges.
This could be done in order to answer an authorization question. 

The report can be fetched via an API call.
The details can also be returned as JWT claim (OIDC) and as SAML attribute.

## Get Person Report via API
In this guide, we will assume the DABP is available under `https://dabp.onegini.com` url. 
The API requires authentication using OIDC. Make sure you add a valid access token to the request header.

The person report for a person with a given identifier (`personId`) can be received by 
doing a GET request to: `https://dabp.onegini.com/api/v1/people/{personId}/report`.

### Example request using CURL

```
curl --location --request GET 'https://dabp.onegini.com/api/v1/people/eb82d9a9-bba5-4bbb-a9fc-508ce6f8705c/report' \
--header 'Content: application/json' \
--header 'Authorization: Bearer  <access token>' \
--header 'Content-Type: application/json'
```

[comment]: <> (# Person report)

[comment]: <> (Person report contains detailed information about person, including assigned policiec, group memberships and permissions in those groups.)

[comment]: <> (To generate a person report you need to call a dedicated API endpoint.)

[comment]: <> (In this guide we will assume the DABP service is available under `https://dabp.onegini.com` url.)


[comment]: <> (## Generating the report)

[comment]: <> (To generate the report you should call ...)
