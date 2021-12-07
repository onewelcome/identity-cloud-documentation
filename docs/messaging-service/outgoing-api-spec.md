# Outgoing API specification

Here you can see the example request bodies that messaging service can execute when you configure it to use Custom API integrations.

## SMS REST integration

If you choose the Custom API integration option for your SMS gateway, the Messaging service will forward the actual message to the specified endpoint by doing
an HTTP POST request. An example request body that will be sent, will look like:

```json
{
  "from": "+987654321",
  "to": "+123456789",
  "renderedTextBody": "Hello from service John!",
  "contentParameters": {
    "name": "John",
    "lastName": "Doe"
  },
  "messageId": "5f02a5c0-ed54-4e09-a41e-620e1296d3ad",
  "userId": "dc36b80e-08d3-40c6-8b1d-db5b2ae4aa4e"
}

```

## E-mail REST integration

If you choose the Custom API integration option for your email gateway, the Messaging service will forward the actual email to the specified endpoint by doing
an HTTP POST request. An example request body that will be sent, will look like:

```json
{
  "fromAddress": "service@onewelcome.com",
  "toAddress": "john.doe@example.org",
  "ccAddresses": [
    "john.doe+1@example.org",
    "joes.friend@example.org"
  ],
  "bccAddresses": [
    "joes.boss@example.org"
  ],
  "replyToAddress": "support@onewelcome.com",
  "subject": "Example email from service",
  "html": "<html><h1>Html email content</h1></html>",
  "text": "Plain text email content",
  "contentParameters": {
    "name": "John",
    "lastName": "Doe"
  },
  "messageId": "5f02a5c0-ed54-4e09-a41e-620e1296d3ad",
  "userId": "dc36b80e-08d3-40c6-8b1d-db5b2ae4aa4e"
}

```
