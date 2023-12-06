# OAuth Documentation

These external teams are called **clients** in this document, and each client is assigned a unique **client_id** and a corresponding **client_secret**.

To access external APIs, a token must be provided in the header of clients' request. This token ensures secure access to
the APIs and prevents unauthorized access to sensitive data. With these APIs, clients can easily incorporate **Tapsi-Pack**'s features into their own products, enhancing their functionality and user experience.

The token should be set in the header with the key name **"authorization"**.

There are two types of tokens:

- Client Secret Token
- User Access Token

To get your token, please contact us at [Tapsi Pack](https://pack.tapsi.ir/landing) to get your own tokens.

![APIs flow](../images/pack-external-apis-flow.png)


[Token Management for Clients](#token-management-for-clients)

   1. [Generate Access Token and Refresh Token using Client ID and PAT](#1-generate-access-token-and-refresh-token-using-client-id-and-pat)

   2. [Access Token Expiry](#2-access-token-expiry)

   3. [Generate a new Access Token and Refresh token using the Refresh token](#3-generate-a-new-access-token-and-refresh-token-using-the-refresh-token)

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
