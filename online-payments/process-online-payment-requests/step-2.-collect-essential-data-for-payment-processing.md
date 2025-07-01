# Step 2. Collect essential data for payment processing

After decoding the initial payment request, you get JSON-formatted order data. Now, you need to get the essential data for processing a transaction and returning the customer to the store once it's completed.

### What happens on the storefront

For now, the customer is still waiting on your `paymentUrl`.

If the app works quickly, all steps between the first redirect to `paymentUrl` and further redirect to the payment page should take less than a second.

### What happens on the backend

Your app must get the data required for processing the transaction from the decoded JSON received from Ecwid. Most likely, you won’t need all of the details as it contains hundreds of fields.

The list of essential details needed for any transaction includes:

* **Order total**: `cart.order.total` field. This is the final order cost for the customer. You shouldn't add any fees at this step.&#x20;
* **Customer’s email**: `cart.order.email` field. Use it to identify customers and send the transaction check or invoice to them.
* **Order ID**: `cart.order.id` field. Unique order identifier in the Ecwid store.
* **Order currency**: `cart.currency` field. Ensure that the store uses one of your supported currencies.
* **Store owner's settings for the app**: `merchantAppSettings` object. Contains the store owner's settings for the app if it has an integrated settings page called Native app.
* Ecwid Store ID: `storeId` field. Unique identifier of the store in Ecwid.
* **Returning URL**: `returnUrl` field. Use it to return the customers to the store after completing the transaction.

{% hint style="info" %}
The `returnUrl` field is unique for every order in every store. It is used to verify specific payment requests and show the “Thank you for order” page to customers after a successful payment.
{% endhint %}

* **Access token**: `token` field. App's secret token that you must use to update the order payment status after completing the transaction.
* **Transaction ID**: `cart.order.referenceTransactionId` field. Unique identifier of the payment request made by Ecwid. Use it to update the order payment status after completing the transaction.

#### Get order total and currency

To find the order total, parse the JSON and get the `total` field value. Make sure you don’t use non-strict regex for it, as there are fields like `subtotal` or `totalAndMembershipBasedDiscount` that contain the “total” text but not the correct order total value.

To find the order currency, parse the JSON and get the `currency` field value. You’ll get a string containing currency in the ISO 4217 format. Check out the full table of currency ISO codes. Example: `"currency":"USD"`

#### Get store ID and order ID in payment requests

To find store and order IDs, parse the JSON and get two field values: `order.id` and `storeId`. Make sure you get the `order.id` field and not any other `id`, as there are IDs for products, discount coupons, and others.

{% hint style="info" %}
Order ID value always has `string` type.
{% endhint %}

Here are the examples of field values:&#x20;

```json
"order.id": "XJ12H"
"order.items[0].id": 140273658
"order.discountCoupon.id": 29567026
```

#### Get customer’s email in payment requests

To find the customer’s email, parse the JSON and get the `email` field value.

#### Get URL for returning customers to the store

Every payment request contains the `returnUrl` field with a unique URL for returning a customer to the store.

{% hint style="info" %}
Make sure to save and use this field for every transaction.
{% endhint %}

Example: `"returnUrl":"https://mystore.com/01234567?clientId=client_id&timestamp=1751294405226&key=abcdefgh"`

#### Check for the store owner's app settings

When your app has a settings page, all store owner's settings saved there will be added to payment requests coming to your `paymentUrl`.

Find them in the `merchantAppSettings` object. The `"key:value"` format will be automatically converted to JSON format of `"param":"value"`.

Example:&#x20;

```json
"merchantAppSettings": {
    "apiKey": "XXX"
}
```
