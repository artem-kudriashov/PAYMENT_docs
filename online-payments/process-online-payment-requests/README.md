# Process online payment requests

When a customer selects a payment method added by your app and clicks the Go to Payment button, Ecwid provides your endpoint called `paymentUrl` with order data and redirects the customer to the same URL.&#x20;

From this moment, your app must handle both storefront and backend processes simultaneously. The following articles cover the main steps in this flow:

[step-1.-decode-and-parse-payment-request-from-ecwid.md](step-1.-decode-and-parse-payment-request-from-ecwid.md "mention")

[step-2.-collect-essential-data-for-payment-processing.md](step-2.-collect-essential-data-for-payment-processing.md "mention")

[step-3.-initialize-the-transaction-from-payment-provider.md](step-3.-initialize-the-transaction-from-payment-provider.md "mention")

[step-4.-place-order-and-return-customer-back-to-the-store.md](step-4.-place-order-and-return-customer-back-to-the-store.md "mention")

