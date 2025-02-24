# QR Registration Example
This shows some capabilities of the OneWelcome Extension Engine and how you might implement a QR registration. The scripts are based on the following flow:

1. User logs in on the website or portal with their credentials
2. Website calls OneWelcome Access on the backchannel endpoint
3. OneWelcome Access triggers the backchannel script for this Identity Provider and returns the result to the website.
4. Website generates a QR code based on the response
5. User scans this QR code with the mobile app
6. Mobile app calls OneWelcome Access on the complete endpoint
7. OneWelcome Access triggers the complete script and returns the result to the mobile app
8. User is logged in with the mobile app if the result is successful

## Backchannel Script

The backchannel script is the first part of QR registration. It is used as a way to interact with the OneWelcome Extension Engine before the actual user gets involved with
their app. You can utilize this script to store data that will be fetched later. In our example, we send a `userId` to be stored and then fetched during the 
complete script. Below is a sample string that sends a `userId` in the requestPayload.

### Example request to the backchannel script:
```json
{
    "data": "{\"userId\":\"exampleUserId\"}"
}
```

### Example Script

```js
function execute(requestPayload){
    var userId = JSON.parse(requestPayload).userId;
    var identifier = java.util.UUID.randomUUID().toString();

    // Store any data you need
    CACHE.store(identifier, userId, 100); //sets TTL to 100 seconds for this key.
    LOG.info("storing userId: {}", userId);

    return {
      status: 2000,
      responsePayload: identifier
    };
}
```

The syntax used in the script above is described in more detail [here](https://docs-single-tenant.onewelcome.net/msp/stable/extension-engine/topics/writing-scripts.html).
It is recommended to use [properties](../technical-app-management/extension-engine/extension-engine-properties.md) for variables that differ per environment, 
e.g. URLs, and/or contain sensitive data, e.g. passwords.

It utilizes the cache and sets a specific TTL. For more information involving the cache and default TTLs, refer to the [cache documentation](https://docs-single-tenant.onewelcome.net/msp/stable/extension-engine/configuration/setup-cache.html).


### Example response
Here is an example response you'd get from the script above. As part of the QR flow, you should parse the identifier and then embed it in the QR code.

```json
{
   "data": "e2048242-085f-4210-93ff-84df1fcd8ce2", 
   "status": 2000
}
```

## Complete Script

The complete script is the second part of QR registration. In this step, the mobile app will allow the user to scan the generated QR code, parse the data 
embedded in it, and then send that on to the SDK. In our example, the identifier json is sent with the complete request so it can be used to fetch the userId 
that was stored earlier. The json below is a sample of what you would need to send to the SDK. The SDK will automatically escape it when it sends the request 
to OneWelcome Access.


### Example String sent to SDK
```
"e2048242-085f-4210-93ff-84df1fcd8ce2"
```


### Example Script
```js
function execute(requestPayload){
    var identifier = requestPayload;
    var userId = CACHE.fetch(identifier);
    LOG.info("retrieved from cache: {}", userId);

    // You may want to delete the entry so the same request cannot be made again
    var status = 2000;
    if (userId){
      CACHE.delete(identifier);
    } else {
      status = 5000;
    }

    return {
      status: status,
      user: {
        id: userId
      }
    };
}
```
