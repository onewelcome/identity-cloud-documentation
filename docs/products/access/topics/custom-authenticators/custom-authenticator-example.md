# Custom Authenticator example scripts

This section contains example scripts for a Custom Authenticator. They show the minimal response to complete the step in the process. The OneWelcome Extension Engine contains documentation for [writing scripts](https://docs-single-tenant.onewelcome.com/msp/stable/extension-engine/topics/writing-scripts.html) with more details about the syntax of the scripts. 

### Registration script

This is an example for registration which accepts all incoming requests. It returns the `requestPayload` from the request as `registrationData` in the response. `registrationData` is saved in OneWelcome Access for future requests to this Custom Authenticator. The (empty) `responsePayload` is returned via OneWelcome Access to OneWelcome SDK. 

The format of the `requestPayload` parameter is defined by the mobile application. It is sent as String parameter to the `execute` function. The script developer is responsible for parsing this String and implement the logic to either register the user or reject the registration.

```js
function execute(requestPayload, userIdentifier) {
  LOG.debug("Registration of Example Custom Authenticator for user: {}", userIdentifier);

  return {
    status: 2000,
    registrationData: requestPayload,
    responsePayload: ""
  };
}
```

### Authentication script

This is an example for authentication. It compares the `requestPayload` from the current request with the `registrationData` that was saved during registration. When it succeeds, it returns a status code 2000 (Success), otherwise 4000 (Failure, user can try again). The (empty) `responsePayload` is returned via OneWelcome Access to OneWelcome SDK.

The format of the `requestPayload` is defined by the mobile application. Both `requestPayload` and `registrationData` are sent as String parameter to the `execute` function. The script developer is responsible for parsing this String and implement the logic to either authenticate the user or reject the authentication.

```js
function execute(requestPayload, userIdentifier, registrationData) {
  LOG.debug("Authentication of Example Custom Authenticator for user: {}", userIdentifier);

  var validUserKey = requestPayload.equals(registrationData);
  var statusCode = validUserKey? 2000 : 4000;
  
  return {
    status: statusCode,
    responsePayload: ""
  };
}
```

## Status codes

The `execute` function of the script is responsible for returning a status that can be interpreted by OneWelcome Access. OneWelcome Access will perform different actions (such as issuing or revoking tokens, etc.) based on the returned status, and will return only selected status codes to the SDK. Extreme care must be taken that they are correct.

There are different ranges of status codes, each with a particular meaning.

* 2xxx: Success
* 3xxx: Configuration error
* 4xxx: Recoverable client error
* 5xxx: Non-recoverable client error

The script should return a status code of 2000, 4xxx, or 5000. A 3xxx status is returned by the OneWelcome Extension Engine when the script could not be executed. 

The following table contains an overview of the status codes and their meaning. 

| Status | Description                                      | Response to SDK
|--------|--------------------------------------------------|----------------
| 2000   | Success                                          | forward
| 3000   | Requested script not available                   | HTTP 500
| 3001   | Script execution failure                         | HTTP 500
| 3002   | Script `execute` function not available          | HTTP 500
| 3003   | Script class not available                       | HTTP 500
| 3004   | Script result object invalid                     | HTTP 500
| 4000   | Failure, user can try again                      | forward
| 4001   | User already registered                          | HTTP 409
| 4002   | User not registered                              | HTTP 404
| 5000   | Failure, cleanup actions should be triggered     | forward
