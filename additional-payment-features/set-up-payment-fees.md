---
description: Learn how to add fees to payment methods in your store.
---

# Set up payment fees

To add payment fees for your payment method, find it in REST API and update its details with a PUT request.

### Step 1. Find payment method ID with API call

Make a GET call to /profile/paymentOptions:

<mark style="color:green;">`GET`</mark> `https://app.ecwid.com/api/v3/{storeId}/profile/paymentOptions?responseFields=id,appClientId`

In response, you’ll see a JSON with payment options:

```json
[
    {
        "id": "13379-1606718590771",
        "appClientId": ""
    },
    {
        "id": "1272790545-1729232856479",
        "appClientId": "custom-app-15695068-1"
    }
]

```

Parse the response and find the one, where the `appClientId` value matches with your app’s client\_id.

### Step 2. Update fees for the payment method with API call

After getting the ID, use it in a second PUT call that adds a fee to the payment method. The fee has two settings: `type` that defines if the fee is added as a percent of the order total (`PERCENT`) or a flat value ( `ABSOLUTE`) and the value itself (`value` field).

Request example:

```json
PUT https://app.ecwid.com/api/v3/15695068/profile/paymentOptions/1272790545-1729232856479

{
    "paymentSurcharges": {
        "type": "PERCENT",
        "value": 10
    }
}
```

In response, you’ll see `200 OK` HTTP status with the response body:

```json
{
    "updateCount": 1
}
```

This means that you’ve successfully added a fee to the payment method.
