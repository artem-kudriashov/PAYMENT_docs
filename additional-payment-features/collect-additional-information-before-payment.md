# Collect additional information before payment

If you need some additional information about the customer or order, you can collect it by adding a required extra field to the first or second checkout step through a JavaScript file assigned to the app.

Learn more:

[Set up self-hosted JS file](https://app.gitbook.com/s/aRJpOy0U8IpbjUfcox4D/get-started)

[Checkout extra fields](https://app.gitbook.com/s/G9n5VxMY9T0Ob3D56PSD/rest-api/checkout-extra-fields/add-checkout-extra-fields-with-javascript)

### Collect info at the checkout

Get started with a base script that adds an extra field at the checkout start. Collected information will be added to the payment request coming to your app’s `paymentUrl`.

Storefront JS code:

```javascript
//  write a function that adds an extra field
var CheckoutExtraFieldAdd = function () {
   window.ec = window.ec || {};
   ec.order = ec.order || {};
   ec.order.extraFields = ec.order.extraFields || {};

   //  define extra field name and its settings
   ec.order.extraFields.ef_for_payment = {  //  the “ef_for_payment” part is the Extra Field ID
      'title': 'Account ID in bonus payment program',  //  text displayed above the input field
      'textPlaceholder': 'ID00001',  //  text placeholder displayed inside input box
      'type': 'text',
      'tip': 'We will add bonuses to your account after the payment',  //  additional text displayed below the input field
      'required': true,
      'checkoutDisplaySection': 'cart'  //  must be `cart` to display extra field at first chectout step
   };

   window.Ecwid && Ecwid.refreshConfig();
}

// call function on the page load
Ecwid.OnAPILoaded.add(function () {
   Ecwid.OnPageLoaded.add(function (page) {
      if (page.type == "CART") {
         CheckoutExtraFieldAdd();  //  must be `cart` to load extra field at first chectout step
      }
   });
});
```

### Receive info in payment request

You can use any of the `extraFields` or `orderExtraFields` objects for collecting extra fields data. The latter offers more details about extra field storefront settings, but they both contain extra field ID (specified in JS file) and value filled in by a customer.

Extra field details in decoded payment request JSON:

<pre class="language-json"><code class="lang-json"><strong>{
</strong><strong>    ...,
</strong><strong>    "extraFields": {
</strong>    "ef_for_payment": "ID123456"
    },
    "orderExtraFields": [
        {
            "customerInputType": "TEXT",
            "title": "Account ID in bonus payment program",
            "id": "ef_for_payment",
            "value": "ID123456",
            "orderDetailsDisplaySection": "",
            "orderBy": "0"
        }
    ],
    ...
}
</code></pre>
