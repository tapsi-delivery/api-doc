# آموزش OAuth

در این آموزش با روش احراز هویت با 
OAuth.
آشنا می‌شویم.

## مقدمه

در طول این آموزش به تیم‌هایی که قصد دارند خدمات تپسی‌پک را به محصول خود اضافه کنند
**client**
گفته می‌شود. 
به هر client یک
**client_secret**
منحصر به فرد اختصاص داده می‌شود.

برای گرفتن 
**client_secret**
درخواست خود را در
[تپسی‌پک](https://pack.tapsi.ir/landing)
ثبت کنید.

<!-- 
برای دسترسی به API های تپسی‌پک، 

To access our external APIs, a token must be included in the header of the client's request. This token ensures a secure
access to the APIs and prevents unauthorized access to sensitive data.

The token should be set in the header with the key name **"authorization"**.

There are two types of tokens:

- Client Secret Token
- User Access Token -->



![APIs flow](../../images/pack-external-apis-flow.png)

## مدیریت توکن

### ۲. تولید کردن Access Token و Refresh Token با استفاده از Client Secret و PAT



برای درست کردن
**Access Token**
و
**Refresh Token**

توسط
Client
،
باید ابتدا یک
**Client Secret**
و
**PAT**
کاربرتان را داشته باشید. 

شما باید در محصولتان ساز و کاری را پیاده سازی کنید که به واسطه‌ی آن، کاربران بتوانند
PAT
ای که مطابق آموزش مرحله‌ی قبل تولید کرده‌اند را به شما بدهند.

Access Token
ای که در ادامه تولید می‌کنید، دقیقا همان دسترسی‌هایی را به شما می‌دهد که کاربر به
PAT
اختصاص داده بوده است.


با ارسال درخواستی مطابق اطلاعات زیر، می‌توانید
Access Token
و
Refresh Token
را بسازید:

URL:
```
https://api.tapsi.cab/api/v1/delivery/external/oauth2/token
```


Method: 
```
POST
```

Request (as **Form URL Encoded**):

| Field      | Type   | Description        |
|------------|--------|--------------------|
| grant_type | String | authorization_code |
| code       | String | User PAT           |

Response:

```json5
{
  "access_token": "String",
  "refresh_token": "String",
  "scope": "String",
  "token_type": "Bearer",
  "expires_in": "Int"
  // زمان باقی‌مانده تا انقضای access_token به  ثانیه
}
```

نمونه‌ی Curl:

```bash
curl --location 'https://api.tapsi.cab/api/v1/delivery/external/oauth2/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer Client Secret' \
--data-urlencode 'grant_type=authorization_code' \
--data-urlencode 'code=User PAT'
```

برای ارسال این درخواست، به این نکات توجه کنید:

حتما body را به فرمت **x-www-form-urlencoded** 
بفرستید و در هدر نیز کلید
`Content-Type`
با مقدار
‍`application/x-www-form-urlencoded`
را اضافه کنید.


### ۳. انقضای Access Token

تا زمانی که
access token
ای که گرفته‌اید منقضی نشده باشد می‌توانید از آن استفاده کنید.
در صورتی که
Client
از
access token
ای استفاده کند که منقضی شده است، با این 
response
مواجه خواهد شد.

```text
HTTP/1.1 401 Unauthorized
Content-Type: text

Jwt is expired
```

### ۴. تولید یک Access Token و Refresh token جدید با استفاده از Refresh token

هنگامی که
access token
منقضی شد،
Client
می‌تواند با استفاده از
Refresh Token 
یک
Access Token
و
Refresh Token
جدید درست کند.
برای این کار دیگر نیاز به 
PAT
و یا تعامل با کاربر ندارید.

به این طریق، کافی‌ست یک بار
PAT
را از کاربر گرفته باشید و پس از آن بدون تعامل دوباره با کاربر می‌توانید به ادامه‌ی استفاده از دسترسی‌تان بپردازید.

برای این کار، کافی‌ست درخواستی مطابق اطلاعات زیر ارسال کنید:

URL: 
```
https://api.tapsi.cab/api/v1/delivery/external/oauth2/token
```

Method: 
```
POST
```

Request (as **Form URL Encoded**):

| Field         | Type   | Description        |
|---------------|--------|--------------------|
| grant_type    | String | refresh_token      |
| refresh_token | String | User Refresh Token |

Response:

```json5
{
  "access_token": "String",
  "refresh_token": "String",
  "scope": "String",
  "token_type": "Bearer",
  "expires_in": "Int"
  // زمان باقی‌مانده تا انقضای access_token به  ثانیه
}
```

نمونه‌ی Curl:

```bash
curl --location 'https://api.tapsi.cab/api/v1/delivery/external/oauth2/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Bearer Client Secret' \
--data-urlencode 'grant_type=refresh_token' \
--data-urlencode 'refresh_token=User Refresh Token'
```

در حال حاضر هر 
refresh token
تا ۴۸ ساعت معتیر است و پس از آن منقضی خواهد شد.
در این صورت باید دوباره از کاربر
PAT
بگیرید و از مرحله‌ی ۲ به بعد را دوباره انجام دهید.