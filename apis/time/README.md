# Available Dates API

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