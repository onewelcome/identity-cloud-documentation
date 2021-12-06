# Email gateway configuration

The Messaging service supports 3 ways of sending email messages out of the box, by integrating with:

- The OneWelcome email gateway
- A SMTP server
- A custom API

## OneWelcome email gateway configuration

The OneWelcome email gateway is the default integration, as all necessary elements are hosted by OneWelcome. To use this email gateway, you need to specify the
sender email address, sender name and reply-to address. Be aware that OneWelcome only allows _verified_ sender email addresses and domains.
Please [contact support](https://onewelcome.atlassian.net/servicedesk/customer/portals) to get your sender email and domain verified.

## SMTP server configuration

If you don't want to use OneWelcome's email gateway, you can configure any other SMTP server to send out emails.

Apart from the sender address, sender name and reply-to address, you need to specify the hostname (or IP address) and port on which SMTP server is accessible.
You can also specify whether the connection should be marked as secure and authentication details if they're required.

## Custom API configuration

The Custom API integration is meant to be used with any service that conforms to the OneWelcome API specification. See
the [API reference](./outgoing-api-spec.md) for details about the format and supported HTTP methods.

To configure the custom API integration, you must specify the URL of the endpoint to which the Messaging service will forward the data. You can additionally
specify the credentials if the endpoint is protected by basic authentication. You can also specify any other additional parameters that will be passed to the
custom endpoint along with the other notification related parameters, like content, sender, receiver, etc.

The Messaging service will issue HTTP POST requests to the specified endpoint with all data required to send out email notifications. The responsibility to
actually send out the message is moved to the custom API handler.
