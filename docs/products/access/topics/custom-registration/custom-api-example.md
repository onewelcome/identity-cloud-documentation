# Example Scripts

The scripts below are executed by the Onegini Extension Engine during Custom Registration.

Refer to the Onegini Extension Engine documentation for help [writing scripts](https://docs-single-tenant.onegini.com/msp/stable/extension-engine/topics/writing-scripts.html). 

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
The complete script requires a `user` object with an `id` property defined to be returned.

```js
function execute(requestPayload) {
  var userId = "userId_" + Date.now();
  return {
    status: 2000,
    user: {
      id: userId
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

| Status | Description                                 
|--------|------------------------------------------------------------------------------------
| 2xxx   | Success                                     
| 4xxx   | Failed, user can try again                                                         
| 5xxx   | Failed, cleanup actions should be triggered and user must start at beginning again 