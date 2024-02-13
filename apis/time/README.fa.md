# سرویس تپسی‌پک: API تاریخ‌های سرویس‌دهی

این
API
لیست نزدیک‌ترین تاریخ‌هایی را بازمی‌گرداند که می‌توان در آن‌ها سفارش ثبت کرد.
هر یک از این تاریخ‌ها، به فرمت
timestamp
بر حسب میلی ثانیه و منطقه‌ی زمانی تهران 
(IRST (Iran Standard Time) UTC/GMT +3:30 hours) 
است.
همچنین در حال حاضر سه تاریخ در این لیست بازمی‌گردند.

این
timestamp
ها در مرحله‌ی بعد برای گرفتن
preview
استفاده خواهد شد.


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

نمونه‌ی
Curl
(احراز هویت با
API Secret Key):

```bash
curl --location --request GET 'https://api.tapsi.cab/api/v1/delivery/external/embedded/available-dates' \
--header 'x-api-secret-key: 6gJqJu-1eIe9ypYzFh3pwtBkDaltr35Y09Z1zQacuzBcWfMAFFZqQgNdb2q_jWc-CU8wQXaUkEvFBpMIJ7_u24xuWoPABRY-_nyEHXreAATlAxrdTh5-64craO8zm8r2' \
--header 'x-encoded-user-id: MTIz' \
--data-raw ''
```

نمونه‌ی
Curl
(احراز هویت با
OAuth):

```bash
curl --location --request GET 'https://api.tapsi.cab/api/v1/delivery/external/embedded/available-dates' \
--header 'Authorization: Bearer theAccessTokenGeneratedUsingCliendIdAndPat'
```

فراموش نکنید که برای استفاده از این اندپوینت باید حتما از یکی از روش‌های احراز هویت استفاده کنید.