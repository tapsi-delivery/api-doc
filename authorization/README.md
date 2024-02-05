# Tapsi Pack API: Authorization Guide

Welcome to the Authorization guide for the Tapsi Pack API.
This guide will provide you with a clear understanding of how to securely authenticate with the Tapsi Pack API
using OAuth and API Secret Key.

## Definitions

---

### OAuth

OAuth is an open-standard authorization protocol which describes how unrelated servers
and services can safely allow authenticated access to their assets without
actually sharing the initial credential.
In OAuth, tokens are used to obtain a new access token upon the expiration of
an initial access token.

For more information about OAuth, please refer to [OAuth 2.0 Documentation](https://oauth.net/2/).

### API Secret Key

An API Secret Key is a secure, randomly generated string that is used to authenticate a user
when accessing an API. It acts somewhat like a password, unique to each user,
that needs to be included in the header of API requests.

## Usages

---

### OAuth Usage

OAuth is typically used when you need to perform actions on behalf of your users.
For example, if your application wants to access and interact with a user's data in
Tapsi Pack, OAuth would be the appropriate method for authentication.
It is a more secure method since it allows the user to control which data is
accessible to the client.

### API Secret Key Usage

On the other hand, using an API Secret Key is best for server-to-server communication,
where there is no need for external server's user interaction.
This method is recommended when the actions performed are on behalf of the application,
not the application's users.

## Differences and Choosing the Right Method

---

The choice between OAuth and API Secret Key depends on the type of user interaction required.

- **User Interaction**:
    - If your application is making requests on behalf of a user, **OAuth** is necessary.
      Users have the opportunity to consent to what data the application can access and what
      actions it can perform.
    - If your application only needs to access your own data,
      an **API Secret Key** is a reliable and simpler choice.


For additional information and a step-by-step guide on implementing OAuth, please refer to the [OAuth Guide](./oauth/README.md).

For instructions on how to obtain and use your API Secret Key, please refer to the [API Secret Key Guide](./api-secret-key/README.md).

---

Please make sure to review your integration needs carefully to choose the most suitable method for your application.
If you have any questions or need further assistance, feel free to reach out to our [support team](https://pack.tapsi.ir/landing).
