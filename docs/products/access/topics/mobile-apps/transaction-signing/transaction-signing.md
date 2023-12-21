# Transaction signing

## Enable transaction signing with mobile authentication

Transaction signing can be enabled for a mobile authentication transaction. For each 
[mobile authentication type](../mobile-authentication/mobile-authentication.md#configure-mobile-authentication-types) it can be set if transaction signing is 
enabled. 

The application that triggers authentication via mobile authentication needs to provide the data to be signed. When fetching the transaction result the
signature and public key which is used will be returned. Based on this information the signature can be validated and stored for future reference. The signature
is also verified by OneWelcome Access, the result of this verification is also returned in the transaction result. For more details about the API please refer
to the [mobile authentication api reference](../../../api-reference/mobile-authentication/mobile-authentication-v4.md#fetch-authentication-result).

>*Note*: Transaction signing should be enabled on [system level](../../technical-app-management/system-features-config/system-features-config.md).


