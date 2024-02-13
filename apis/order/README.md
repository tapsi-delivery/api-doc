# Order APIs

## Order Preview API

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
      "invoice": "Nullable Invoice"
      // invoice is null when the timeslot is not available
    }
  ]
}
```

* Invoice

    ```json5
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
  "sender": "{Sender}",
  "receiver": "{Receiver}",
  "pack": "{Pack}",
  "timeslotId": "string",
  "token": "string"
  // the provided token in the preview response
}
```

* Location
  ```json5
  {
    "coordinate": {
      "longitude": "float",
      "latitude": "float"
    },
    "description": "string",
    "buildingNumber": "string",
    "apartmentNumber": "string"
  }
  ```
* Phone
  ```json5
  {
    "number": "string",
    "type": "Case Sensitive String"
    // HOME, MOBILE, OTHER
  }
  ```
* Receiver
  ```json5
  {
    "fullName": "string",
    "phones": ["{Phone}"],
    "location": "{Location}"
  }
  ```
* Sender
  ```json5
  {
    "location": "{Location}"
  }
  ```
* Pack
  ```json5
  {
    "description": "string" // Every description that is provided by the user when submitting the order
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

| Field    | Type   | Description |
|----------|--------|-------------|
| order_id | String |             |

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
        "phoneNumber": "+98911111111",
        "name": "رضا رضایی",
        "phones": [
          {
            "number": "+98911111111",
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
  "request_items": ["{RequestItem}"],
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

---

[![fa](https://img.shields.io/badge/lang-fa-greed.svg)](./README.fa.md)
