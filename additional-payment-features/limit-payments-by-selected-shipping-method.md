---
description: >-
  Learn how to make your payment method work only with specified shipping
  methods.
---

# Limit payments by selected shipping method

When customers go through the checkout, they choose their preferred way of shipping or pickup for the order before they can select a payment option. Ecwid allows you to prevent your payment method from appearing with unwanted shipping/pickup methods in the store.

For example, you can make your online payment work only with shipping methods, and not with in-store pickup. If customers select a pickup option, they won’t be able to complete the order with your payment method.

To add this limitation, first you need to find your payment method ID with REST API call. And then update shipping limitations for the payment method with a PUT request.

### Step 1. Find your payment method ID with API call

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

### Step 2. Get shipping/pickup method IDs with API call

Make a GET call to /profile/shippingOptions:

<mark style="color:green;">`GET`</mark> `https://app.ecwid.com/api/v3/{storeId}/profile/shippingOptions?responseFields=id,title,fulfilmentType`&#x20;

In response, you’ll see a JSON with all shipping and pickup options available in the store:

```json
[
    {
        "id": "39809685-1728985376955",
        "title": "Easyship: Shipping and Logistics Solutions",
        "fulfilmentType": "shipping"
    },
    {
        "id": "6589-1709547151586",
        "title": "Standard shipping",
        "fulfilmentType": "shipping"
    },
    {
        "id": "4959-1595934622523",
        "title": "Pickup",
        "fulfilmentType": "pickup"
    }
]
```

Parse the response and save IDs of shipping methods that your payment option should work with. All other shipping methods will prevent your payment option from appearing at the checkout.

### Step 3. Set allowed shipping methods for your payment method with API call

After getting the payment option ID and IDs of shipping options, use them in a PUT request that adds (or overrides any existing) shipping limitations for the payment method.

You need to send an array of allowed shipping methods in the following format inside request body:

```json
{
    "shippingSettings": {
        "enabledShippingMethods": [
            "3919-1640004025851",
            "4959-1595934622523"
        ]
    }
}
```

Request example:

```json
PUT https://app.ecwid.com/api/v3/15695068/profile/paymentOptions/1272790545-1729232856479

{
    "shippingSettings": {
        "enabledShippingMethods": [
            "39809685-1728985376955",
            "6589-1709547151586"
        ]
    }
}
```

In response you’ll see `200 OK` HTTP status with the response body:

```json
{
    "updateCount": 1
}
```

Now your payment method works only with specified shipping methods at the checkout.
