# Step 3. Initialize the transaction from payment provider

With all the required details, you can process the transaction.

#### What happens on the storefront

When a customer selects your payment method and clicks the **Go to Payment** button at the checkout, Ecwid redirects a customer from the store checkout to your `paymentUrl`.&#x20;

After that, you show the customer a placeholder while processing the initial payment request. Once that part is finished, you have two options:

* If possible, initialize the payment form right on the `paymentUrl` . It reduces the number of redirects and simplifies the checkout process for customers.
* Otherwise, initiate a transaction and redirect a customer to an external payment page hosted by the payment provider.&#x20;

#### What happens on the backend

Independantly on the storefront approach you chose, you must ensure that the app will receive the transaction status to its `paymentUrl`, and keeps the essential order details to complete the payment process while waiting.
