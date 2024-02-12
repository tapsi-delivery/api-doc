# Tapsi Pack API: API Secret Key Guide

Welcome to the API Secret Key guide for the Tapsi Pack API. This guide will help you understand how to authorization
with the
Tapsi Pack API using API Secret Key

## Introduction

API Secret Key is a randomly generated string that is assigned to each user. This key is used to authenticate the user
when accessing the Tapsi Pack API.

Each user can have a maximum of **one active API Secret Key**.

To access our external APIs, two headers must be included in the user's request. These headers ensure secure
access to the APIs and prevent unauthorized access to sensitive data:

- `x-api-secret-key` : User API Secret Key
- `x-encoded-user-id` : User ID encoded with Base64

To obtain your personal API Secret Key and Encoded User ID, please contact us
at [Tapsi Pack](https://pack.tapsi.ir/landing).

![Authorization flow](../../images/pack-api-secret-key-flow.png)

## API Secret Key Generation Process

When users contact us to integrate with our APIs, we generate an API Secret Key for them and send it to them. The API
Secret Key
has the following features:

### 1. API Secret Key Scopes

The API Secret Key can have a custom scope. This feature is useful when the user wants to limit the access of their API
Secret Key to a specific set of features.
So when the user wants to limit the access of their API Secret Key to a specific set of APIs, they can clarify the scope
of their API Secret Key when contacting us.

Currently, the available Scopes are defined [here](/apis/README.md#overview).

### 2. Expiration Feature

The API Secret Key can have a custom expiry date, but the default expiry duration is 1 year. This feature is useful when
the user wants to limit the validity of their API Secret Key.

### 3. API Secret Key Note

The API Secret Key can have a custom note. This feature is useful when the user wants to add a note to
their API Secret Key in order to ease the process of managing their API Secret Key.
We recommend to set the note of the API Secret Key related to the user's usage.

## Using the API Secret Key

Note that the `x-encoded-user-id` field is the User ID encoded with Base64.
For example if the userId is **123**, the encoded userId will be `MTIz`.

Here is a sample cURL request to use [Available Dates API](/apis/time/README.md) using API Secret Key:

```bash
curl --location --request GET 'https://api.tapsi.cab/api/v1/delivery/external/embedded/available-dates' \
--header 'x-api-secret-key: 6gJqJu-1eIe9ypYzFh3pwtBkDaltr35Y09Z1zQacuzBcWfMAFFZqQgNdb2q_jWc-CU8wQXaUkEvFBpMIJ7_u24xuWoPABRY-_nyEHXreAATlAxrdTh5-64craO8zm8r2' \
--header 'x-encoded-user-id: MTIz' \
--data-raw ''
```

## API Secret Key Revocation Process

The API Secret Key can be revoked and a new one can be generated for the user. This feature is useful when the user
suspects that their API Secret Key has been compromised.
To revoke your API Secret Key and generate a new one, please contact us.

---

[![fa](https://img.shields.io/badge/lang-fa-greed.svg)](./README.fa.md)
