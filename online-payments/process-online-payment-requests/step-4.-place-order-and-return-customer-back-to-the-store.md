# Step 4. Place order and return customer back to the store

Once the transaction is complete and your app has its status, you need to update the order payment status in Ecwid and send the customer back to the store.

### What happens on the storefront

After a customer completes the transaction and your app updates the order payment status in the store, you need to return customers back to the store.

{% hint style="warning" %}
Update the order payment status first!
{% endhint %}

The app must use the unique `returnUrl` received in the initial payment request. This way, Ecwid can verify the payment and the customer and show the order confirmation page.

Congratulations! :tada:

The order is now placed in the store, the payment is complete, and the customer gets the order details.

#### How to handle failed transactions

In case of a failed transaction, first, you need to complete the backend part – update the order payment status to incomplete. Then, return customers to the storefront with an error message.

To do so, take the `returnUrl` from the payment request received from Ecwid, add an `errorMsg=text` query parameter, and redirect a customer to the resulting URL. The text itself must be URI-encoded, for example: `https://example.com/123456?clientId=client_id&hash=ABC&errorMsg=Payment%20error`

A customer will be redirected back to the final checkout step (with all of the cart details as before the payment) and will see a notification displaying your error message.

You can also add several translations for error messages. In the payment request coming from the store, there is the `lang` field that contains the current storefront language in ISO 639-1 format.

### What happens on the backend

When customers complete payments, your app must update the order payment status in the Ecwid store. It also must happen before redirecting customers back to the storefront.

Updating the order payment status after a transaction requires a special API call.&#x20;

To make it, you need the following details from the initial payment request you received on `paymentUrl`:

* Transaction ID assigned by Ecwid: `cart.order.referenceTransactionId` field.
* Ecwid store ID: `storeId` field.
* Access token for the store: `token` field.

Other than that, you need to know if the transaction was successful.&#x20;

{% tabs %}
{% tab title="Successful transaction" %}
Set the `PAID` payment status to order.&#x20;

Request example:

```http
PUT /api/v3/ORDERID/orders/transaction_id HTTP/1.1
Host: app.ecwid.com
Authorization: Bearer secret_token
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

{
    "paymentStatus": "PAID"
}
```

Change `ORDERID`, `transaction_id` and `secret_token` with the values saved earlier to make the example work for your payment requests.&#x20;
{% endtab %}

{% tab title="Failed transaction" %}
Set the `INCOMPLETE` payment status to order.&#x20;

Request example:

```http
PUT /api/v3/ORDERID/orders/transaction_id HTTP/1.1
Host: app.ecwid.com
Authorization: Bearer secret_token
Content-Type: application/json;charset=utf-8
Cache-Control: no-cache

{
    "paymentStatus": "INCOMPLETE"
}
```

Change `ORDERID`, `transaction_id` and `secret_token` with the values saved earlier to make the example work for your payment requests.&#x20;

{% hint style="danger" %}
There is the `CANCELLED` payment status, which we don't recommend using as it puts an incomplete order in the “Orders” list in the store.
{% endhint %}
{% endtab %}
{% endtabs %}

That's it! The order is now placed in the store.

However, you need to redirect customers back to the store so they know about it too.
