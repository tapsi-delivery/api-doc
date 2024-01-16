# Tapsi Pack API: Authorization Guide

Welcome to the Authorization guide for the Tapsi Pack API. This guide will help you understand how to authenticate with the Tapsi Pack API using OAuth and API Secret Key.

## Overview

In this document, external teams integrating with our API are referred to as **clients**.

The Tapsi Pack Authorization process is handled by two different methods:
- **OAuth**: This method is used to authenticate **users** and generate access tokens.
- **API Secret Key**: This method is used to authenticate **clients**.

### OAuth

OAuth is a more complex, but also more secure and flexible method of authentication. 
It involves multiple steps, including obtaining an access token and refresh token, 
and optionally using the refresh token to obtain a new access token when the old one expires.
When using OAuth, the orders are submitted and paid by the user, so the user is responsible for the payment.

For more details, please refer to the [OAuth Guide](./oauth/README.md).

### API Secret Key

API Secret Key is a simple static randomly generated string that is used to authenticate a client when accessing the Tapsi Pack API.
It is a straightforward method of authentication where the key is passed in the header of the request.
When using API Secret Key, the orders are submitted and paid by the client, so the client is responsible for the payment.

For more details, please refer to the [API Secret Key Guide](./api-secret-key/README.md).

## OAuth API Secret Key: Which One Is Right for You?
It's important to understand the difference between API Secret Key and OAuth.
API Secret Key is a simpler method where the `client` submits
and pays for orders.
On the other hand, OAuth is a more secure and flexible method of authentication,
where the `user` submits and pays for orders.
