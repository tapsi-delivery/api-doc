# Error Models Documentation

[Error Models](#error-models)

   1. [Error Message](#error-message)

   2. [Error Details](#error-details)

   2.1 [Error Localization](#error-localization)

   2.2 [Constraints of Error Details](#constraints-of-error-details)

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
