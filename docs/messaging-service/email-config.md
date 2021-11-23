# E-mail Gateway configuration

Messaging service supports 3 ways of sending E-mail messages out of the box:

- By integrating with the Onewelcome email gateway
- By integrating with custom SMTP server
- By integrating with custom API

## Onewelcome email gateway Integration configuration

Onewelcome email gateway is the easiest integration, as all necessary elements are hosted by Onewelcome. 
To use this email gateway you just need to specify the sender email address, sender name and reply-to address.
Be aware that Onewelcome verifies if the sender email address and domain. Please [contact support](https://onegini.atlassian.net/servicedesk/customer/portals) to get your sender email and domain verified.

## SMTP server configuration

If you don't want to use Onewelcome email gateway you can configure any other SMTP server to send out emails.

Apart from sender address, sender name and reply-to address you just need to specify the hostname (or IP address) and port on which SMPT server is accessible.
You can also specify if the connection should be marked as secure and authentication details if they're required.

## Custom API configuration

Custom API integration is meant to be used with any service that conforms to the required API specification.
See [API Reference](./outgoing-api-spec.md) for details about the format and supported HTTP methods.

To configure custom API integration you must specify the URL endpoint to which Messaging Service will forward the data.
You can additionally specify the basic auth credentials if the endpoint is protected by basic auth.
You can also specify any other additional parameters that will be passed to the custom endpoint along with the other notification related parameters (like content, sender, receiver etc.)

Messaging service will issue POST HTTP requests to specified endpoint with all data required to send out e-mail notification. The responsibility to actually send out the message is moved to the custom API handler.