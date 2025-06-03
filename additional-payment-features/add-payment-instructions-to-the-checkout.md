---
description: >-
  Learn how to add payment instructrions to your payment method at the store
  checkout.
---

# Add payment instructions to the checkout

Use payment instructions to guide the customers through the next steps when they select your payment method at checkout. Or explain any fees your payment requires.

To add instructions, first you need to find your payment method ID with REST API call. And then to update instruction details for the payment method with a PUT request.

### Step 1. Find payment method ID with API call

Make a GET call to `/profile/paymentOptions:`

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

After getting the payment option ID, use it in a second PUT call that adds an instruction to the payment method. The fee has two main settings: title and the instruction text in the following format:

```json
{
    "instructionsForCustomer": {
        "instructionsTitle": "Payment instructions",
        "instructions": "Pay easily with the following steps: ",
        "instructionsTranslated": {
            "en": "Pay easily with the following steps: ",
            "nl": "Betaal eenvoudig met de volgende stappen:"
        }
    }
}

```

The `instructionsTranslated` object is optional, it’s only required if your store is multilingual and you want to add translations for the instruction. The first “language: value” pair here must match the main store language and text in the \`instructions\` field.

Request example:

```json
PUT https://app.ecwid.com/api/v3/15695068/profile/paymentOptions/1272790545-1729232856479

{
    "instructionsForCustomer": {
        "instructionsTitle": "Need any help?",
        "instructions": "Pay easily with the following instructions: …"
    }
}

```

In response you’ll see \`200 OK\` HTTP status with the response body:

```json
{
    "updateCount": 1
}
```

Now your payment method has customer instructions visible at the store checkout.\
