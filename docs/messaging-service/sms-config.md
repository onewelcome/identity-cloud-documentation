# SMS gateway configuration

Messaging service supports 2 ways of sending SMS messages out of the box:

- By integrating with the [CM.com SMS gateway](https://www.cm.com/sms/)
- By integrating with a custom API

## CM.com integration configuration

The default integration for the Messaging service is with CM.com. CM.com requires
a [product token](https://www.cm.com/app/docs/en/api/business-messaging-api/1.0/index#integration-authentication) to send SMS messages via its gateway.
Please [contact support](https://onewelcome.atlassian.net/servicedesk/customer/portals) to obtain a product token from CM.com if you do not have one. To use it,
this product token must be specified in the configuration screen.

You can also specify the default sender, which will be used as a sender of the SMS message when the request to send the message does not contain a sender. This
default sender can be a phone number, a short code or the name of the sender.

## Custom API configuration

The Custom API integration is meant to be used with any service that conforms to the OneWelcome API specification. See
the [API reference](./outgoing-api-spec.md) for details about the format and supported HTTP methods.

To configure the custom API integration, you must specify the URL of the endpoint to which the Messaging service will forward the data. You can additionally
specify the credentials if the endpoint is protected by basic authentication. You can also specify any other additional parameters that will be passed to the
custom endpoint along with the other notification related parameters, like content, sender, receiver, etc.

The Messaging service will issue HTTP POST requests to the specified endpoint with all data required to send out SMS notifications. The responsibility to
actually send out the message is moved to the custom API handler.
