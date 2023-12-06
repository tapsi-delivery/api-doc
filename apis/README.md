# Tapsi Pack API: Client APIs Guide

Welcome to the Client APIs guide for the Tapsi Pack API. This guide will help you understand how to use the public APIs provided for clients.

## Overview

The Tapsi Pack API provides a set of public APIs that can be used by clients. The access token should be set in the header of all APIs with the key name "authorization". This guide covers the following APIs:

1. **Available Dates API**: This API returns a list of timestamps that correspond to the first days in which Tapsi Pack can offer services in the Tehran timezone.

2. **Order Preview API**: This API allows clients to preview the prices for their orders.

3. **Order Submission API**: This API allows clients to submit their orders.

4. **Get Order API**: This API allows clients to retrieve information about their orders.

5. **Change Order Status API (Cancellation)**: This API allows clients to cancel their orders.

## [Available Dates API](time/README.md)

The Available Dates API returns a list of timestamps that correspond to the first days in which Tapsi Pack can offer services in the Tehran timezone. These timestamps are used in the preview API to calculate time-slots for a specific day. This API can be called with both client secret and user access token.

## [Order Preview API](order/README.md)

The Order Preview API allows clients to preview the prices for their orders. If this API is called using the client secret instead of the user access token, you can display a preview of prices to your users without the need for a user token. The only difference is that the Token field in the response of this API will be an empty string.

## [Order Submission API](order/README.md)

The Order Submission API allows clients to submit their orders. If this API is called using the client secret instead of the user access token, you can display a preview of prices to your users without the need for a user token. The only difference is that the Token field in the response of this API will be an empty string.

## [Get Order API](order/README.md)

The Get Order API allows clients to retrieve information about their orders. Clients can select an order by the ID of the order.

## [Change Order Status API (Cancellation)](order/README.md)

The Change Order Status API (Cancellation) allows clients to cancel their orders. This method allows clients to cancel one or multiple orders.

For more details on each API, please refer to the respective sections in the documentation.