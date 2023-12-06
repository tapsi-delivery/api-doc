# Client APIs Documentation

[Client APIs](#client-apis)

   1. [Available Dates API](#available-dates-api)

   2. [Order Preview API](#order-preview-api)

   3. [Order Submission API](#order-submission-api)

   4. [Get Order API](#get-order-api)

   5. [Change Order Status API (Cancellation)](#change-order-status-api-cancellation)

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
