# SMS Gateway configuration

Messaging service supports 2 ways of sending SMS messages out of the box:

- By integrating with the [CM SMS gateway](https://www.cm.com/sms/)
- By integrating with custom API

## CM Integration configuration

CM SMS gateway integration is based on [CM Product token](https://www.cm.com/app/docs/en/api/business-messaging-api/1.0/index#integration-authentication)
To use it, a product token must be speficied in the configuration screen.
You can also specify the `Default sender` which will be used as a sender of the SMS message. `Default sender` can be a phone number, a short code or the name of the sender.

## Custom API configuration

Custom API integration is meant to be used with any service that conforms to the required API specification.
See [API Reference](./outgoing-api-spec.md) for details about the format and supported HTTP methods.

To configure custom API integration you must specify the URL endpoint to which Messaging Service will forward the data.
You can additionally specify the basic auth credentials if the endpoint is protected by basic auth.
You can also specify any other additional parameters that will be passed to the custom endpoint along with the other notification related parameters (like content, sender, receiver etc.)

Messaging service will issue POST HTTP requests to specified endpoint with all data required to send out SMS notification. The responsibility to actually send out the message is moved to the custom API handler.
