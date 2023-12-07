# Tapsi Pack API: Error Models Guide

Welcome to the Error Models guide for the Tapsi Pack API. This guide will help you understand the error handling mechanism used in the Tapsi Pack API.

## Overview

The Tapsi Pack API uses the [Google Error Model APIs](https://cloud.google.com/apis/design/errors) as the underlying infrastructure for its error handling. Each error that occurs during the handling of a request in Tapsi Pack follows a specific format:

```json5
{
  "code": "Int",
  // A simple error code that can be easily handled by the client.
  "message": "String",
  "details": [
    {
      "@type": "type.googleapis.com/google.rpc.LocalizedMessage",
      "locale": "fa-IR",
      "message": "مختصات داده شده اشتباه است یا تپسی پک در این مختصات سرویس ارائه نمی‌دهد."
    }
  ]
}
```

## Error Message

The error message is designed to help developers understand and resolve the API error quickly and efficiently. The message field is intended for developers and is written in English.

## Error Details

The Tapsi Pack API provides standard error payloads for error details. These payloads cover the most common needs for API errors. Developers should use these standard payloads whenever possible. Currently, Tapsi Pack has one known payload:

| Field             | Type   | Description                                                                                                             |
|-------------------|--------|-------------------------------------------------------------------------------------------------------------------------|
| ErrorLocalization | String | If a user-facing error message is needed. We use this error payload, in which the message in this payload is localized. |

### Error Localization

If a user-facing error message is needed, the LocalizedMessage will appear in the details field.

Error Localized Message:

```json5
{
  "@type": "type.googleapis.com/google.rpc.LocalizedMessage",
  "locale": "fa-IR",
  "message": "مختصات داده شده اشتباه است یا تپسی پک در این مختصات سرویس ارائه نمی‌دهد."
}
```

### Constraints of Error Details

The Tapsi Pack backend team guarantees that the error details will always be one of the standard error payloads. However, the backend team does not guarantee that the error details will always be present. For example, if the error is caused by a network failure, the error details may not be available. In this case, the client should not rely on the error details to be present. Instead, the client should rely on the error code and message to be present. The client should also be prepared to handle the case where the error details are not present.

Also, The Tapsi Pack backend team guarantees that the list of error details always contains one or zero of each unique type, and there are rules for handling them:

- If there are any ErrorLocalization, the message in this object should be shown to the end user.