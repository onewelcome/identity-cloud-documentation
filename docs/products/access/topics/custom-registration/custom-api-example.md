# Example Scripts

The scripts below are executed by the Onegini Extension Engine during [Custom Registration](./index.md).

Refer to the Onegini Extension Engine documentation for help [writing scripts](https://docs-single-tenant.onegini.com/msp/stable/extension-engine/topics/writing-scripts.html). 
It is recommended to use [properties](../technical-app-management/extension-engine/extension-engine-properties.md) for variables that differ per environment, 
e.g. URLs, and/or contain sensitive data, e.g. passwords.

#### Init Script
```js
function execute(requestPayload) {
  return {
    status: 2000,
    responsePayload: ""
  };
}
```

#### Complete Script
The complete script requires a `user` object with a defined `id` property to be returned. The `amr` property is optional (requires 
extension-engine `2.5.0` or later). The `user` object may also contain any other attributes describing the authenticated user, these attributes are exposed in the `id_token` as well as via the UserInfo and Token introspection APIs.

```js
function execute(requestPayload) {
  var userId = "userId_" + Date.now();
  return {
    status: 2000,
    user: {
      id: userId,
      amr: ["mfa", "pwd"],
      firstName: "John",
      lastName: "Doe",
      email: "john.doe@onewelcome.io"
    }
  };
}
```

#### Backchannel Communication Script

```js
function execute(requestPayload) {
  var userId = requestPayload.userId;
  var uniqueIdentifier = userId+"_"+Date.now();
  CACHE.store(uniqueIdentifier, userId); //Another script could use CACHE.fetch(...) to retrieve the stored value
  return {
    status: 2000,
    responsePayload: uniqueIdentifier
  };
}
```


## Status Codes
When writing a custom script, these status code ranges should be used.

| Status | Description                                                                        |
|--------|------------------------------------------------------------------------------------|
| 2xxx   | Success                                                                            |
| 4xxx   | Failed, user can try again                                                         |
| 5xxx   | Failed, cleanup actions should be triggered and user must start at beginning again |
