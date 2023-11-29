# Tapsi Pack API Documentation

# Table of Contents

1. [Token Management for Clients](#token-management-for-clients)

   1.1 [Generate Access Token and Refresh Token using Client ID and PAT](#1-generate-access-token-and-refresh-token-using-client-id-and-pat)

   1.2 [Access Token Expiry](#2-access-token-expiry)

   1.3 [Generate a new Access Token and Refresh token using the Refresh token](#3-generate-a-new-access-token-and-refresh-token-using-the-refresh-token)

2. [Error Models](#error-models)

   2.1 [Error Message](#error-message)

   2.2 [Error Details](#error-details)

   2.2.1 [Error Localization](#error-localization)

   2.2.2 [Constraints of Error Details](#constraints-of-error-details)

3. [Client APIs](#client-apis)

   3.1 [Available Dates API](#available-dates-api)

   3.2 [Order Preview API](#order-preview-api)

   3.3 [Order Submission API](#order-submission-api)

   3.4 [Get Order API](#get-order-api)

   3.5 [Change Order Status API (Cancellation)](#change-order-status-api-cancellation)

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

# Token Management for Clients

## 1. Generate Access Token and Refresh Token using Client ID and PAT

Each client is assigned a Client ID, known as the **"Client ID"**. Clients have the ability to generate both an **Access
Token** and a **Refresh Token** using their **Client ID** in combination with the **user's PAT**. The Access Token that
is generated inherits the permissions associated with the corresponding PAT. The body of the message should be sent
as **x-www-form-urlencoded**, and the provided **client secret** should be set as the **Bearer** authorization header.

URL:

```
/api/v1/delivery/external/oauth2/token
```

Method:

```
POST
```

Request:

(as **Form URL Encoded**)

| Field      | Type   | Description          |
|------------|--------|----------------------|
| grant_type | String | authorization_code   |
| code       | String | User PAT             |

Response:

```json5
{
  "access_token": "String",
  "refresh_token": "String",
  "scope": "String",
  "token_type": "Bearer",
  "expires_in": "Int"
  // seconds to expiration
}
```

Curl Example:

```bash
curl --location 'https://api.tapsi.cab/api/v1/delivery/external/oauth2/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer Client Secret' \
--data-urlencode 'grant_type=authorization_code' \
--data-urlencode 'code=User PAT'
```

The client can utilize the Pack APIs by using the **access token**. The available APIs are described in the Client APIs
part of this document. The **access token** should be placed in the request header using the **`authorization`** key.

## 2. Access Token Expiry

Clients are allowed to utilize the access token until it reaches its expiration point. Should a client attempt to use an
access token that has already expired, the following response will be generated.

```text
HTTP/1.1 401 Unauthorized
Content-Type: text

Jwt is expired
```

## 3. Generate a new Access Token and Refresh token using the Refresh token

Once a client has obtained an initial Access Token and Refresh Token, they can use the Refresh Token to generate a new
Access Token and Refresh Token without needing to involve the user's PAT or Client ID again. This is useful for
maintaining continuous access to the API without frequent user interactions. To generate a new set of tokens using a
Refresh Token, clients should make a POST request to the token endpoint, and the body of the message should be sent
as **x-www-form-urlencoded**, and the client secret as the **Bearer** authorization header.

URL:

```
/api/v1/delivery/external/oauth2/token
```

Method:

```
POST
```

Request:

(as **Form URL Encoded**)

| Field          | Type   | Description         |
|----------------|--------|---------------------|
| grant_type     | String | refresh_token       |
| refresh_token  | String | User Refresh Token  |

Success Response:

```json5
{
  "access_token": "String",
  "refresh_token": "String",
  "scope": "String",
  "token_type": "Bearer",
  "expires_in": "Int"
  // seconds to expiration
}
```

Error Response:

```text
HTTP/1.1 401 Unauthorized
Content-Type: text
 
Jwt is expired
```

Curl Example:

```bash
curl --location 'https://api.tapsi.cab/api/v1/delivery/external/oauth2/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer Client Secret' \
--data-urlencode 'grant_type=refresh_token' \
--data-urlencode 'refresh_token=User Refresh Token'
```

At present, the access token is configured to remain valid for a duration of 2 days. When it expires, users need to log
in using their Personal Access Token (PAT) again, following the same process as in Step 1.

# Error Models

This document provides an overview of the Tapsi-Pack error model for Istio APIs, which
uses [Google Error Model APIs](https://cloud.google.com/apis/design/errors) as the underlying infrastructure. Every
error that occurs within handling a request in Tapsi-Pack that is called has the following format:

```json5
{
  "code": "Int",
  //A simple error code that can be easily handled by the client.
  "message": "String",
  "details": [
    {
      Error
    }
  ]
}
```

## Error Message

The error message should assist users in comprehending and resolving the API error quickly and easily. The message field
is intended for developers and is written in English.

## Error Details

There are some standard error payloads for error details. These cover the most common needs for API errors. Like error
codes, developers should use these standard payloads whenever possible. For now, in Tapsi-Pack we have one known
payload:

| Field             | Type   | Description                                                                                                             |
|-------------------|--------|-------------------------------------------------------------------------------------------------------------------------|
| ErrorLocalization | String | If a user-facing error message is needed. We use this error payload, in which the message in this payload is localized. |

### Error Localization

If a user-facing error message is needed, use LocalizedMessage which will be appeared in the details field.

Error Localized Message:

```json5
{
  "@type": "type.googleapis.com/google.rpc.LocalizedMessage",
  "locale": "fa-IR",
  "message": "مختصات داده شده اشتباه است یا تپسی پک در این مختصات سرویس ارائه نمی‌دهد."
}
```

### Constraints of Error Details

The Tapsi-Pack backend team guarantees that the error details will always be one of the standard error payloads.
However, the backend team does not guarantee that the error details will always be present. For example, if the error is
caused by a network failure, the error details may not be available. In this case, the client should not rely on the
error details to be present. Instead, the client should rely on the error code and message to be present. The client
should also be prepared to handle the case where the error details are not present.

Also, The Tapsi-Pack backend team guarantees that the list of error details always contains one or zero of each unique
type, and there are rules for handling them:

- If there are any ErrorLocalization, the message in this object should be
  shown to the end user.

# Client APIs

These APIs can be used by clients. The access token should be set in the header of all APIs with the key name "
authorization".

## Available Dates API

This API returns a list of timestamps that correspond to the first days in which Tapsi Pack can offer services in the
Tehran timezone. These timestamps are used in the preview API to calculate time-slots for a specific day. This API can
be called with both client secret and user access token.

URL:

```
/api/v1/delivery/external/embedded/available-dates
```

Method:

```
GET
```

Response:

```json5
{
  "availableDatesTimestamp": [
    "1690576200000",
    "1690662600000",
    "1690749000000"
  ]
}
```

## Order Preview API

Clients are allowed to utilize the access token until it reaches its expiration point. Should a client attempt to use an
access token that has already expired, the following response will be generated. Also, if this API is called using the
client secret instead of the user access token, you can display a preview of prices to your users without the need for a
user token. The only difference is that the Token field in the response of this API will be an empty string.

URL:

```
/api/v1/delivery/external/embedded/order/preview
```

Method:

```
GET
```

**Query Params:**

| Field           | Type   | Description |
|-----------------|--------|-------------|
| originLat       | Float  |             |
| originLong      | Float  |             |
| destinationLat  | Float  |             |
| destinationLong | Float  |             |
| dateTimestamp   | Float  |             |
| couponCode      | String | (optional)  |

Response:

```json5
{
  "token": "string",
  "invoicePerTimeslots": [
    {
      "timeslotId": "string",
      "startTimestamp": "Long",
      // start timestamp of the timeslot
      "endTimestamp": "Long",
      // end timestamp of the timeslot
      "isAvailable": "bool",
      // indicate if the timeslot is available or not
      "invoice": {
        Invoice
      }
      // invoice is null when the timeslot is not available
    }
  ]
}
```

```json5
// Invoice
{
  "discount": "int",
  // the amount of discount
  "amount": "int",
  // the total amount of the order
  "paymentInAdvance": "int",
  // the payment in advance amount (amount - discount - senderCredit)
  "descriptions": [
    // the detail of the invoice provided as a list of title and description
    {
      "title": "string",
      // description title according to the user language
      "amount": "int"
    }
  ]
}
```

## Order Submission API

Clients are allowed to utilize the access token until it reaches its expiration point. Should a client attempt to use an
access token that has already expired, the following response will be generated. Also, if this API is called using the
client secret instead of the user access token, you can display a preview of prices to your users without the need for a
user token. The only difference is that the Token field in the response of this API will be an empty string.
URL:

```
/api/v1/delivery/external/embedded/order/submit
```

Method:

```
POST
```

Request:

```json5

{
  "sender": {
    Sender
  },
  "receiver": {
    Receiver
  },
  "pack": {
    Pack
  },
  "timeslotId": "string",
  "token": "string"
  // the provided token in the preview response
}
```

```json5
// Location:
{
  "coordinate": {
    "longitude": float,
    "latitude": float
  },
  "description": "string",
  "buildingNumber": "string",
  "apartmentNumber": "string"
}
```

```json5
// Phone:
{
  "number": "string",
  "type": "string"
  // HOME, MOBILE, OTHER
}
```

```json5
// Receiver:
{
  "fullName": "string",
  "phones": [
    {
      Phone
    }
  ],
  "location": {
    Location
  }
}
```

```json5
// Sender:
{
  "location": {
    Location
  }
}
```

```json5
// Pack:
{
  "description": "string"
}
```

Response:

```json5
{
  "orderId": "string"
  // the submitted order identifier
}
```

## Get Order API

Clients can select an order by the ID of the order.

URL:

```
/api/v1/delivery/external/embedded/order/{order_id}
```

Method:

```
GET
```

**Parameters:**

| Field      | Type   | Description |
|------------|--------|-------------|
| order_id   | String |             |

Example Response:

```json5

{
  "order": {
    "origin": {
      "coordinate": {
        "latitude": 35.630649566650391,
        "longitude": 51.364894866943359
      },
      "description": "تهران، بل شکوفه، خ. سبلان",
      "buildingNumber": "۱۶۱",
      "apartmentNumber": ""
    },
    "receiver": {
      "location": {
        "coordinate": {
          "latitude": 35.631649017333984,
          "longitude": 51.363895416259766
        },
        "description": "بلوار شکوفه، سبلان",
        "buildingNumber": "3",
        "apartmentNumber": ""
      },
      "fullName": "محمد محمدی",
      "phoneNumber": "09121111111",
      "phones": [
        {
          "number": "09121111111",
          "type": "MOBILE",
          "isPrimary": true,
          "title": "شماره همراه"
        }
      ]
    },
    "package": {
      "description": ""
    },
    "timeslot": {
      "startTimestamp": "1697121000000",
      "endTimestamp": "1697131800000",
      "id": "1697121000000_1697131800000"
    },
    "createdAt": "1697037952899",
    "orderCode": "PACK-680212",
    "orderId": "65r12ea7db",
    "price": {
      "price": "14500",
      "passengerShare": "14500"
    },
    "statusInfo": {
      "status": "CANCELLED_BY_SENDER",
      "title": "لغو شده توسط فرستنده",
      "description": "سفارش توسط فرستنده لغو شد.\nدر صورت نیاز، می\u200cتوانید با پشتیبانی تپسی\u200cپک به شماره ۱۶۳۰ تماس بگیرید."
    },
    "cancelable": false,
    "rating": {},
    "sender": {
      "id": "54759226",
      "profile": {
        "firstName": "رضا",
        "lastName": "رضایی",
        "phoneNumber": "+989301234567",
        "name": "رضا رضایی",
        "phones": [
          {
            "number": "+989301234567",
            "type": "MOBILE",
            "isPrimary": true,
            "title": "شماره همراه"
          }
        ]
      },
      "location": {
        "coordinate": {
          "latitude": 35.630649566650391,
          "longitude": 51.364894866943359
        },
        "description": "تهران، بل شکوفه، خ. سبلان",
        "buildingNumber": "۱۶۱",
        "apartmentNumber": ""
      }
    },
    "proofOfDelivery": {
      "required": true,
      "code": "9600"
    },
    "nonTaker": {
      "type": "RECEIVER",
      "phones": [
        {
          "number": "09121111111",
          "type": "MOBILE",
          "isPrimary": true,
          "title": "شماره همراه"
        }
      ],
      "locations": [
        {
          "location": {
            "coordinate": {
              "latitude": 35.631649017333984,
              "longitude": 51.363895416259766
            },
            "description": "بلوار شکوفه، سبلان",
            "buildingNumber": "3",
            "apartmentNumber": ""
          }
        }
      ],
      "name": "محمد محمدی"
    }
  }
}
```

## Change Order Status API (Cancellation)

This method allows clients to cancel one or multiple orders.

URL:

```
/api/v1/delivery/external/embedded/order/change-status/cancelledBySender
```

Method:

```
PUT
```

Request:

```json5
{
  "request_items": [
    {
      RequestItem
    }
  ],
}
```

```json5
// RequestItem
{
  "order_id": "string",
  "cancelled_by_sender_metadata": {}
}
```

Response:

```json5
{}
```
