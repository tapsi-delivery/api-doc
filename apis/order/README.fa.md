# سرویس تپسی‌پک: API های سفارش

## پیش‌نمایش

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
| couponCode      | String | (اختیاری)  |

Response:

```json5
{
  "token": "string",
  "invoicePerTimeslots": [
    {
      "timeslotId": "string",
      "startTimestamp": "Long",
      // timestamp لحظه‌ی شروع بازه‌ی زمانی
      "endTimestamp": "Long",
      // timestamp لحظه‌ی پایان بازه‌ی زمانی
      "isAvailable": "bool",
      // مشخص می‌کند که آیا می‌توان سفارشی در این بازه‌ی زمانی ثبت کرد یا نه
      "invoice": "Nullable Invoice"
      // هنگامی که نتوان سفارشی در یک بازه‌ی زمانی ثبت کرد، این مقدار null خواهد بود
    }
  ]
}
```

* Invoice

    ```json5
    {
      "discount": "int",
      // مقدار تخفیف به تومن
      "amount": "int",
      // کل هزینه‌ی سفارش
      "paymentInAdvance": "int",
      // مقداری که باید پرداخت شود تا اعتبار کاربر برای ثبت سفارش کافی شود (کل هزینه‌ی سفارش منهای تحفیف منهای اعتبار کاربر)
      "descriptions": [
        // جزئیات صورتحساب که لیستی است از عنوان‌ها و هزینه‌ی مربوط به هر عنوان
        {
          "title": "string",
          // عنوان هزینه
          "amount": "int"
          // مبلغ مربوطه
        }
      ]
    }
    ```

نمونه‌ی curl:

```bash
curl --location 'https://api.tapsi.cab/api/v1/delivery/external/embedded/order/preview?originLat=35.69013977050781&originLong=51.34545135498047&destinationLat=35.6959114074707&destinationLong=51.49193130493164&dateTimestamp=1707769800000' \
--header 'Authorization: Bearer theAccessTokenGeneratedUsingCliendIdAndPat'
```

نمونه‌ی Response:

```json
{
    "token": "aTokenInThePreviewResponseThatShouldBeUsedOnOrderSubmission",
    "invoicePerTimeslots": [
        {
            "timeslotId": "1707888600000_1707899400000",
            "startTimestamp": "1707888600000",
            "endTimestamp": "1707899400000",
            "isAvailable": false
        },
        {
            "timeslotId": "1707899400000_1707910200000",
            "startTimestamp": "1707899400000",
            "endTimestamp": "1707910200000",
            "isAvailable": false
        },
        {
            "timeslotId": "1707910200000_1707921000000",
            "startTimestamp": "1707910200000",
            "endTimestamp": "1707921000000",
            "isAvailable": false
        },
        {
            "timeslotId": "1707921000000_1707931800000",
            "startTimestamp": "1707921000000",
            "endTimestamp": "1707931800000",
            "isAvailable": true,
            "invoice": {
                "discount": 0,
                "amount": 74000,
                "paymentInAdvance": 0,
                "descriptions": [
                    {
                        "title": "مبلغ سفارش",
                        "amount": 74000
                    },
                    {
                        "title": "موجودی کیف پول",
                        "amount": 357045
                    },
                    {
                        "title": "پرداخت از کیف پول",
                        "amount": 74000
                    }
                ]
            }
        }
    ]
}
```

## ثبت سفارش

به دو شکل می‌توان از این
API
استفاده کرد.
در حالت اول، ثبت کننده‌ی سفارش همان فرستنده‌ی بسته است. در این حالت، در
Sender
کافی‌ست تنها
Location
را بنویسید و تپسی‌پک نام و شماره تماس ثبت کننده‌ی سفارش را به عنوان نام و شماره تماس فرستنده‌ی بسته ثبت می‌کند.

اما حالتی را در نظر بگیرید که فرستنده‌ی بسته، کسی غیر از ثبت‌کننده‌ی سفارش باشد.
در این شرایط، باید در
Sender
علاوه بر
Location،
مقدار
Profile
را نیز بفرستید که شامل شماره تماس و نام فرستنده‌ی بسته است.


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
  // توکن دریافت شده در پاسخ preview 
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
    "isPrimary": "bool",  // Can only be true for one of the phones.
    "number": "string",
    "title": "string",
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
    "location": "{Location}",
    "profile": {
      "name": "string",
      "phones": ["{Phone}"]
    }
    // optional
  }
  ```
* Pack
  ```json5
  {
    "description": "string" // توضیحات و یادداشت‌هایی که به راننده نمایش داده می‌شود
  }
  ```

Response:

```json5
{
  "orderId": "string"
  // شناسه‌ی یکتای سفارش
}
```

نمونه‌ی curl:

```bash
curl --location 'https://api.tapsi.cab/api/v1/delivery/external/embedded/order/submit' \
--header 'Authorization: Bearer theAccessTokenGeneratedUsingCliendIdAndPat'
--header 'Content-Type: application/json' \
--data '{
    "token": "aTokenInThePreviewResponseThatShouldBeUsedOnOrderSubmission",
    "timeslotId": "1707921000000_1707931800000",
    "package": null,
    "sender": {
        "location": {
            "coordinate": {
                "latitude": 35.69013977050781,
                "longitude": 51.34545135498047,
                "bearing": null
            },
            "description": "تهران، ب یادگار امام، بعد از خ. سعادت آباد، بل بهزاد، ک. باغستان",
            "buildingNumber": "33",
            "apartmentNumber": ""
        }
    },
    "receiver": {
        "fullName": "خانم یا آقای گیرنده",
        "phoneNumber": "09123456789",
        "location": {
            "coordinate": {
                "latitude": 35.6959114074707,
                "longitude": 51.49193130493164,
                "bearing": null
            },
            "description": "تهران، ب یادگار امام، بعد از خ. سعادت آباد، بل بهزاد، ک. باغستان",
            "buildingNumber": "2",
            "apartmentNumber": ""
        }
    }
}'
```

## گرفتن اطلاعات سفارش

به کمک شناسه‌ی هر سفارش،
client
ها می‌توانند جزئیات سفارش را بگیرند

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

نمونه‌ی Response:

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

نمونه‌ی curl:

```json
curl --location 'https://api.tapsi.cab/api/v1/delivery/external/embedded/order/654a26f63b7f106a1ce40efa' \
--header 'Authorization: Bearer theAccessTokenGeneratedUsingCliendIdAndPat'
```

## تغییر وضعیت سفارش (لغو سفارش)

این متد به
client
این اجازه را می‌دهد تا یک یا چند سفارش را لغو کند.

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

نمونه‌یcurl:

```bash
curl --location --request PUT 'https://api.tapsi.cab/api/v1/delivery/external/embedded/order/change-status/cancelledBySender' \
--header 'Authorization: Bearer theAccessTokenGeneratedUsingCliendIdAndPat' \
--header 'Content-Type: application/json' \
--data '{
    "request_items": {
        "order_id": "65cb7edfccfd4c3282875835"
    }
}'
```

---

[![en](https://img.shields.io/badge/lang-en-red.svg)](./README.md)