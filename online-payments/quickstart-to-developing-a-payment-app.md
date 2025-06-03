---
hidden: true
---

# Quickstart to developing a payment app

[Sample payment app](https://github.com/Ecwid/payment-gateway-template) contains several files covering its essential features:

* **payment-request.php** file contains the code for decoding and processing payment requests from Ecwid stores. Essentially, it’s your `paymentUrl` endpoint. Every time a customer clicks the “Go to Payment” button at the checkout, Ecwid sends a new payment request that the `paymentUrl` must process.
* **index.html** (optional) file provides a user settings page for the app. Store owners can access this page in Ecwid admin and fill in their settings, like API keys or account credentials required for connecting the payment gateway.
* **functions.js** (optional) file that allows saving user settings in the App Storage hosted on Ecwid side. Only used with **index.html**.

{% hint style="info" %}
If you build an integration for one store, then you can simplify the process even more by only using the **payment-request.php** file. \
\
Any credentials and other variables for connecting the payment gateway can be added to the file.
{% endhint %}

The sample app is hosted on GitHub.&#x20;

[**Go to GitHub to get the code**](https://github.com/Ecwid/payment-gateway-template)
