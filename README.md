# Tapsi Pack API Documentation
# Get started

**Tapsi-Pack** offers a set of embedded APIs for external teams to integrate **Tapsi-Pack** functionalities into their
own applications. These external teams are called **clients** in this document, and each client is assigned a unique **
client_id** and a corresponding **client_secret**.

To access external APIs, a token must be provided in the header of clients' request. This token ensures secure access to
the APIs and prevents unauthorized access to sensitive data. With these APIs, clients can easily incorporate **
Tapsi-Pack**'s features into their own products, enhancing their functionality and user experience.

The token should be set in the header with the key name **"authorization"**.

There are two types of tokens:

- Client Secret Token
- User Access Token

To get your token, please contact us at [Tapsi Pack](https://pack.tapsi.ir/landing) to get your own tokens.

![APIs flow](images/pack-external-apis-flow.png)

API Endpoint:

```
https://api.tapsi.cab/
```