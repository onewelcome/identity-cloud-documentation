# Identity Providers

As an organization you want to give your end-users the freedom to choose how they want to sign in to your applications. You can do that with (federated) Identity Providers (IdP). These pages will provide you with a basic understanding of how various Identity Providers connect to the OneWelcome Identity Cloud.

An Identity Provider is a system that creates, maintains, and manages identity information and then provides authentication services to your applications. Identity Providers can significantly reduce sign-in and registration friction, which allows your users to easily access applications without needing to create new passwords or remember usernames. It can also provide you with some level of certainty about the real-life identity of your end-user.

## Overview

The OneWelcome Identity Cloud manages connections to various Identity Providers for your application and sits between your application and the Identity Provider that authenticates your end-users.

- When you use the OneWelcome Identity Cloud as the user store for your applications, users can sign in with their email and password by default.
- You can add connections to social Identity Providers like Google, Twitter, or Facebook. It allows your users to sign in to your app using credentials from their social accounts. After users authenticate, you sync certain attributes from their social account to an Identity in your Onegini user store, while continuing to use that Identity Provider for user authentication. This eliminates the need to store an additional username and password for that user.
- You can add connections to predefined federated Identity Providers that are active in our primary markets. The following link contains a [complete list of the Identity Providers that we support](https://www.onegini.com/knowlegde/apis-integrations#identity_proofing).
- You can add connections to Identity Providers that you have built in-house. As long as these support the OpenID Connect or SAML protocols. This is also sometimes referred to as inbound federation or inbound SAML. The SAML flow is initiated with the service provider (in this case, Onegini) that redirects the user to the Identity Provider for authentication. After authentication, a user is created inside your Onegini user store, and the user is redirected back to your application. As the OneWelcome Identity Cloud allows you to connect your applications via SAML and OIDC, we can also function as a proxy between these protocols for cases where you have (legacy) applications that are incompatible with one of these protocols.

Adding any of these Identity Providers allows users to sign in to your application using their credentials from a specific Identity Provider.

## Benefits of using the OneWelcome Identity Cloud to manage Identity Providers

You could connect your application directly to an Identity Provider. However, using the OneWelcome Identity Cloud as the user store for your application and letting Onegini manage the Identity Provider connections has some benefits:

- **No custom code:** Your application only needs to talk to the OneWelcome Identity Cloud.
- **One protocol:** Your application can use SAML, OpenID Connect, or both to talk to the OneWelcome Identity Cloud. Onegini handles whatever protocols the other Identity Providers uses. This prevents a lot of implementation effort on your side.
- **Single user store:** All users are stored in the OneWelcome Identity Cloud. You can capture the profile attributes from an Identity Provider user and store those attributes in your Identity Store.
- **Profile sync:** If a user updates their profile at the Identity Provider, those changes can be reflected inside the OneWelcome Identity Cloud the next time that they use the provider to sign in.
- **Account linking:** A single end-users can use multiple Identity Providers to sign in, and the OneWelcome Identity Cloud links those profiles to a single user in your Onegini user store.
- **Identity Proofing:** The OneWelcome Identity Cloud allows you to store a score that represents how sure you are about the real-life identity of an end-user. This gives you a complete overview of how and when a user provided information about their real-life identity.
