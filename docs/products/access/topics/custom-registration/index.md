# Custom Registration

Onegini Access has support for user registration via Custom API Identity Providers.

## Custom API Identity Providers

### One Step

A one step API will only call the `complete` script. This type of Identity Provider can be used to validate on a single request by the user such as a 
username and password. It will run the `complete` script and return `status` and any custom `data` that is defined in the script.

### Two Step

A two step API utilizes the `init` and `complete` scripts that have been defined for the Identity Provider in the admin console. The `init` endpoint will
respond with a `status` and any custom `data` that has been defined in the script.

### Backchannel Communication
Refer to the [Custom Registration Backchannel Communication API](../../api-reference/backchannel-custom-registration.md) reference guide for more
information about backchannel communication.

### Identity Provider Config
See [Configure a Custom API Identity Provider](../general-app-config/identity-providers/identity-providers.md#configure-a-custom-api-identity-provider) for 
more information on configuration.

## Example Requests and Responses

### Init Request
```json
{
   "data": "{\"custom_json_key\":\"custom json data\"}" // optional
}
```

### Init Response
```json
{
    "status": 2000,
    "data": "{\"custom_response_key\":\"custom_response_data\"}" //optional
}
```

### Complete Request
```json
{
    "data": "{\"custom_key\":\"custom_data\"}"
}
```

### Complete Response
```json
{
    "status": 2000,
    "data": "{\"custom_response_key\":\"custom_response_data\"}" //optional
}
```

## Status Codes
When writing a custom script, these status code ranges should be used.

| Status | Description                                 
|--------|------------------------------------------------------------------------------------
| 2xxx   | Success                                     
| 4xxx   | Failed, user can try again                                                         
| 5xxx   | Failed, cleanup actions should be triggered and user must start at beginning again 