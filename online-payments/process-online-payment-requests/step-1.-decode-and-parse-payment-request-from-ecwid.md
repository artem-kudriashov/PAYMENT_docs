# Step 1. Decode and parse payment request from Ecwid

When a customer proceeds to payment from the Ecwid checkout, Ecwid API sends a unique payment request to the endpoint of an app assigned to the chosen payment method.

After receiving a new payment request from Ecwid on your `paymentUrl` endpoint, the first step is to decode and parse it. All payment requests are encoded and contain all the order data, which is essential for further steps.

### What happens on the storefront

Upon clicking the “Go to Payment” button, a customer is redirected to your app `paymentUrl`. For now, you can’t do anything with the storefront, so we recommend showing a loading placeholder until further steps.

### What happens on the backend

The `paymentUrl` endpoint of your app receives a POST request with encoded data in the request body.

<details>

<summary>Encoded request example</summary>

{% code overflow="wrap" %}
```html
<form action="https://mycoolapp.com/integration" method="POST" accept-charset='utf-8'>
<input type='hidden' name='data' value="2sO2fxaXOZz9aZMxwuwvEpFSBFKd6yPTeyk3R3V6ieXIDc9romK5vpTv9kTCoMZVmxJd8kabS-bEJy7ON6vzpG4X8QMb3CYhl_WF2T7VAX883FfMverEo4jY9VlEUS9ZSJuuBVeUPnG5FE-bLTPNrMbWFXltyzt3cz5mHu8ha50oPXPDJ7mcihSH-H-i32LLTvwkfgqJa7CNd3n4XM985B1smvJEw4o6acsy1RO62eJVWJZ7k1_UTfa5S3AhrmcY0OxZuTQ3pTVVG0uKsnB73eZMhijPkPlx5Rehu_9IaMylrwOy2E15hzTnmAYActRgFVGSH8NevL9lGnTifYTp6thUqJn1HhiFjIvNQveEHIPIq5n-uOn9wkodVoDeEsuin_96t_eP5uJUFjHEpnXsDdtJpE9BVzCJrDEU2wprzYCGdc_p28f9J6jEyH4Ld2FrF0kRv_GacJwIKghGQixu_BlTlrBO1DZ1B3AhBS2IHmZVCj_U2Ux9s20-5ZTgJRgsMyeObj1I8byMPc29GGXXSJVbYfcWjk8PGtcToQu9tSFV-zYur9VB7T8ewdxst-_kTnnAPmIdKFfxHpaRIu3GYVpubytnvOMhsIuoeN3lmZ0scX_leX6w4ZE1iduEKQT3GWopZoHBTKF1-441PDY4MemDDSfUTHqkvr9LrOC4gxvXlB2EddTqUfpFFX1QZX_jpvxttQp0RXwyIi5WDVJ0pU6b-De69-QHkzeaaLbbTzJvpzkKDOmQq_TNgZnPY7sdCLpQ8ECgadWxdIxmRGCY8s-HP_sOB8Pbp5T_BHHnr8Dzk6qFcay_MHy8IVn8kP0T3USxTBkdr44KTgq-5jZa77SojW9CwvaA-GqdIRxJwjabvWPGhYsiZBxJu8pyiYDHCadc9WeXAzk5yILd9peyp0ADZAHS5l4f7jnMJOfqElTqfSg_Xq16OwX74IQBx_Isu4swQkQX4whAcaJTDo0AsE2AkQJKXjkdqxfFUtDPyMwW6wmcK7DD81xmybcOVpxU0ZiA12L-Gg8Pczgr7GeUYUVm3aLQu5IbP_o2B1woLivNaT-fvs7ssw1LiRzGkFfsBdWhJkJe7g9OWW3QKs1T91IxWGRNLMA55UIxNXVYKLsOOSKQRUKH0KUe6llAR4ptYJoLl0fULwuGZjXdTePTu791tZ-0tPZrychVMHsvqCIvdFlMVQ9C50q8deRkozQ8YHbKu0ih6nKB2xDQ6dDhS-GPz5cQn-2xZFASZHkniqvzJvA6Iy-NMU-IYpnTXzleQixN4fyQMSAkQTOfnMmg54vvFL0esX4LTBXgRx58hqzxXDM3PffUznq2bbhS_ey8ytRM9gfXcNKLTSwdHtX89Uh_pez7mgAqTnp2RiaLyN-hsJryO3xnlPj-HoTgA_jZz_yxB9jaNBO5fLbDuWdftIVPJ9wTjrSgwvUYM0esD-QZvtrAx__drHeovLgsSZwPsZAdbQdY912NH30xTAYsuZISGB2stEiY3-jZMKTPbuWHAzyiWCrgOwEVuAAW9HgptffCkD28dAkqjPnJUaRMLzjzG1lrtAcolhr1TNzI3-qoKNMawvoN73-Mu1hshd3W81MyQSVVN4A1Mq46RA0QgGby2L26YcJ2zgJvbAwWFCk4Z4yLEOzwwRJvnmJ213ncBYAMKAlOgRijKHWaEWOBxztPQxP6WMjXrbAXIJhB_V8uLHkPYI3HO_5ntpLjHv8iPtRpRInZKqXLDinkA2Oez3EAj0gctLtsRCAPxFnC2dFABP_nIbogsaAkoWwXLdPVcp-CT3mtsuFo9Lrnbjp2T9aGAF9Ni0EAYDn1JI1hKmSLa_9jPnFzg5TDxbIBCS6UVfgD0oahf0fVOTChwaig8L6n92qJDhzpZ2YkBGIMgmp15F65OzNrpblRk0GFxdxKVpvetvWqqD4bT5BDoima5qqwnqvxiGp3kn_Y-oj5W3m98pT7DfjUALQzSwwmmHPsowFqdpBxrCcCqeoS2KgdvCn02w5vaYeOjCTr64hUHYzvMKhTS77U2JOp6lYU5009CSssK9mdQM134qW53d3uy_pnjD980rcvv0vbrjTN1qQYQFYOzggkgHLMqyELVZz8NPqoBO7NKyFXlevhkhm3vOMQdaILCVv-QVrLsAA3jwSLeWIeG9a2dL8xJyLj6qRoMzZ6TAEw4jfXAdNi3u6ap6AmyN8H2i0UTce6HrtEY92iPWXi-fEe9eLRkFZKbaNQK_1oEFLs6W_Owd3LTlXUIDqTxYXpxPOEMz0XjjYKa3BZhWES1xMVXsk9IP03GI3DNzKhJOBNJrsBn1Wn-qFB-bmnAvdUVVQeR9tdOYkcfqRiyOYAlBN4fijuO2Spz1s2jmZ0qF7XFjb_5YovtBnCnyQmz-HSNLvfo3Db_WEw1Yq56mozW4ztsRExQK4Mnq6IoTlDYJHE0DMGYeRO1tIV-aMqnlnBkeu8Rgs5zA3pPTuZvrzLd_2fNyI9HkOzr5cQ_ebaXrFHzs4NVdYVcZbogPysMwD1INj6_gAQdsvvgWt22XUTIOCNX8ZtTx-kCk8__zMTeEr0foXEO9JTupdCletliNAVJ7zbGQBj_q8o9Pc_7uOZo3rhv37Tc1plVcmyQDCWKh3iAsoOgZTFW0R7AWhigqtueH14m8IruynORJ6l1Y3ubr1JmCN1DzqHWxS7k04Sdp7zjfpU-_aQgslGTp7M7hJksm3v7lrvy8cr26zzAiSqtbab6pIpXJfbGCRBJb4Y7i_gc3ljJvmwLPmFY_mQ6AsCwKmzE1LEKrtOUm8BbJlecpwjH0ig-ynGX9nHs_j8_-jQilsHbstJ_a9O5HuuxJfQPPSTfkN0R7NUwoFZH9kxer1Y_QNC37yFalfZIB-z2i1h9ughCE4bYsoorFb2bTVb_R0DH42ZvwES9AAWLifa8v1p-uDVyPtFjk1k6wbaTp8tWdrU3T8MVY0fPVaxE1vdiBdHHcXwnEz-XplQsAd3FHVlowoqGeNHs9FKAv779MZGKa1O49IQOY2TLadd6Hcqwzm10DXB81Re92-pPROQSNGb7Vt876s0IO71k8j0aTD2GaqQlmoZ6GJJFzpK6yMEE-n4SFAsIIj9aEipeVwblroAYYN_ma2-5Gr02-fX9vKHby6TgDq71Z2stYQheUWvUVvcPiyWijrkdWhrYc3SeloJdJeVvhEMYsJ3IgZ3sSBtpRX-5vccpjN03LBih_PCQXQVWdQEAv8bTpMU6uDL804f09-86WFXu54v4p2pqPT-s214Hvnjm6jLZYJRksOOPxAduMSrDTz8JGidezE3n7-F3jZnFSNb6Juv5HXfRhxxJ2WGtgm2SUIvsJSVPWHKlE8PkrGOISjg7m0EUfD-ypzz3Fnv7sWnkfGg7HNwr69gWb--9dadxSUbWy82twCnaMm3OKpsW3ZQ__HE1_NPCpfyc-vW90hkj_hYa7Bpcq3jlY-5tTG6dqe8UNGdH7sKqVnYM98-VRuEBMj3ihQW_hfujErqWCo5JD748IV0Orz1yNo0NSt7kBqKJK9gSGkPDc5M9qS_EkIwIH7PLVpWItSuPvgvE8jso4qf-ywHzevlbcM7uBFBIaMhiRxYh33U6u02zUgT42ZFMPe3dV4SkIIFxqufnsmWn-iw2EszIoJrhNN_bllGikZ-sodTS66D5ygscvI-NqOTmoqjaV3jKMmppj42Rxe6rMTx2u9MiifRxJlg2iLHE273wJmLu5_RWKRNJpoWgeqFRQ9iuWrAyV72BwNeYbk2AI5tL74iiI567CgpdSmHwC7VUAV9F8AztFTq0kt1uTpIvf129ytt6l5hlvlDPFvvksG7dUFEMcryevPoJNo4rghBxY5Ru7MNb1FME3RGgk2pXI4nRPjOOFfEG8WyOK6TfseJSRXNhBH0irQnukRROCB1NXR9dq35TlXGT-L05oSYUaoiSomOC-J9qC3FkWglzRs3ZuDXPP8uab0v_rN3CSJYX8_nnlbu-VUo6A3BCEIVUsiZfi6Vrpb3-BpvCkbA_fXTo1kIzERK82OD3UYfcragvSFH8TrtbmuusnY_NTUOYuiylYStUjCT0ohXyxf8fStd60fNqFLR4Oa-g7IZf3cf8JdJelFiMF99XNdD3TrApnMpAq8QE01_bCAL77PZGWEsPQRcqGR6zFRhJ3a_H0KPhrlSwvcZ6XtZUvbPskj-PjwbDtLcvuCOSaKe-JEbN4Ljk_8G6sZgTJy_tSGCqVcgHCjbio7oN_Q0609cZ6nAOIwjKrmzKrdc086ALFWhebI3dIjV80YJays7clppo9rDv-WrmOH1-IOh7cneew8MhiQAzE_GA32oYimXRShBahMrbfSSC24WuJYDpixr38OUa8CQZdDmMD2j9pHziAtY5tEiVenmZM7OPiHy3Ywb_gHQk0Re_lo-LT1kocMShMJxCaS949oDkO6TM1kbkmAbVSqlrLzJzIG2lYqixmTH3JSmPZZsUrtzggp8uhGttozGyEgsvpvvcCCZ453YBrN1KylPY116f1VhI02FEOXBb3zUHuYyGPuO9ux7en50jMWWPu9npljgOL37b28JY0WX_Md5TOEnQa0xxEEygIQ0OZUpThOX8ySEr6ArFfrmd5T4ZYzlNiyK0W6z6JTg6_YDAYo7Xesg7AJvhow36vffbeyVN9QgJULywade_7xmJGSsqaOX_mrmqe_J5jSdXvn2eqWHbeMAx7J4tW7dzhuAbOK4k8AJcty1C0UJ5xQqlTGlvNRnqfSCG__w__lg7yF3e35DBSmJjAqVjkuVnYOaL4YMnDTEjWsaVrPsYM51Zl4nBAmW5IRwthwPCONovaw4ME_oR1xWp01iPF5ntntijBg1A1PgDVy_hPnb4boJMb2CAXElqpT-2AtgTok8hJtbM6mhZ6hZHlsZwHW16CJgqGbnuBI0UWvstrQu02WrgJHykbTLG2nkk1m59ytCh1cPmxFWAehikumciywABuwWN51r9HzMmgjdoVQh5ht68Qp9tyrgNc0HAEQmlkjqeopw8MF60vd5CMEStcOZwYkJOVSta26oKb0wkHz1M5PVDy1s922xha0I71N06VykH270dMa1NPdU-zC-OCJFDMCURiiIjFBWkigfKZ5M9rJKOJFNwtNEJZi3BI2VIwmlEjSPc6cDz3ayDmluaVToe6vsko4awBrD7OYrVRwxaZGsdxEzQ_wg3uaxPZkcBMChwoFBMpx_LLtCXv_H04urW8Ej6Dh3-fWZWwwtR7HDYNvrWCZZyunpAW3R5UTh18qBul5Xn5eW_A5vDHR2P64-Ve5GoI_Ey5cZ4KLGsD84AqjsayotHlucUtfNuMTG_Cb_27ja3SzMxGOpCRpQ-lVXutOVhy6yItmXSWuxNGmoQTaFlP2rH5VbAuO-4o4flCGDd6GW7Q2g58UTVggn7PqcSlbMSgnPvjApEqzTzHjlLct5kJk7w_-VuLO-JSmgascIWUWJKMBYz0kTyjs7N4_MeDiR52myYExUQt8g-d5UAT0xxlRhcYfMvHFRE2TQ-gGEuOIDCCdKJaEo-NTsb8IQ_dPz-VSog33o1y1UUQ24D_wsG8m5dhbMc2HVXZOgRb1jBYxiW8WTpMjaV6zHM7xFmyLV-EuWivHZmME1Mf1b4vayfV0TE9xDEZix8Omixln-Tg8G4iPqvlBwVcLVqjhvOi-iiolasL65VuO2w8JoA7XdAezLgkmmZoeiQUbqVyWEBeepF8lH-zEvABtivbSurZqk_aj2IQo-cyOZmd3n9s_q3jv-uYU6j-VNXAH8w41bmUWTGJeHrc-Xv_94eMCwxNNTyxS72FspNIqHVtoPklFyLi_Yp_Q7451NiZixX5ixAmdLaQEdcs6cYuVzp4zpnA3xB0GX1uueGY04llv50z0G8u3zjy4g5bF9vFUrZZHmdWZnFXlPOHsMX_deIOXow3rSdH7RD096nq5NSX5D8c99QdnQU6QMKrp6T-llbAdD5h9_e-8Px6N8ofEBxn50ukKYzblv51_q5e1wxo4qqNjmSDGttmQk3OJyQgvKRg6z1t343jjcx-LwKZilHV87__pkfpzXiACYlwPQUtU5FMOU-P2Au7zLbdof-7PZVSGk8JG3yifoZasz66cDEdgSgZw0yno8Z-bR9ukecfOFhRSCoz-dOeislyl8_Hr_emOinGLu2pNAuNrFonKrcek7arqNUgtyWZtrncbkGGEUG2OasoWkHxgyylwqNcEhcBCqQDX9XGXFTNretk4VfSjasMcPMS_N4o3jJhFnxMQMV0AXjksK3OpWVIljGDYjlGJqrb_Ils7tkFd5T-F8fcwKoTMSp7a8AOt4gRMrmS9mUeSbgzRJTjYlTr0a-0aI7mhejw47aO8eGI1GPP5zyUykgOmfnFny1uT5QrZ6mEHogLfP-0aecdOu5beutfTQM-VZYxDeq9KXKvSI5LljDM67D3W5UPdWDBEfJFZaKvBIxfgXzAFFrR7lXzzseEyYU_VkZb1yQy9O7YsKgageFg9P6UC0MkgDkguldH8BWKWq7dSqpNiIXzJ1tgpMz5xQKSkQHS-WSIIby10rgUvTpclxfqbyv-q1SPY558VSf-vwM5KZzvpKNxMP6YP-zYn_P3uqLPV7sEECDRVjs4Ti3xWjGJJ7IfvMm7T5dTii9YdSBVJ1dOJ19NpLZG6IeWczt1j2tbalO9D1ouhkyG2SZOfPtYZmIjgxwpB5eCOixPGA4rhh7ejRLQBLZ3BHhoPAq3e9bPmSUWjioYefytDGGlf7DvgP3y8bGTVyrtT7iEY7ut0xHMFa4rcYAlOW8WmMvo0PDbQjptz6fzvYawrSgUaJ6zF7VhecDhEuvs6tM1JtB4BD64uugpsJQfvQXM_nkrZArApYvFhHYTLqKaOVN7bUa17eHukScPTqcSJqpWGTecL9MBvroHMr8LJckXH9UKjZruoItLpGTjiIQVYTn03zqu54XclIPh2q5q5rCnltj7MAgxOyVGACj4PDOt-Q5riiTk-A-zmm9qYeMLx40UI1psE9cre8wrGURve9pOtjbKU5s6jRfgqrbU5CEm4aBnJZUiku8cBQkP_4HqqltzlCiAD40tsR--eDo5M0F1nqneMOOTL5V8kDn1V38Qh96Pwv50esv5JWxLA-gBI5YWBfiQGuoVSfXi1coRtPkquXmvJL6o-UhcO0KYOBwzkNrx9IZBWbKLLrxoMf6vSQwRk2sL8CgjCSX6nf9XaoQ17Ji0_TSICxIBo-tfz0pXq_x8Nq_lqgATk747QSBqWkj3KjGimAQYhWU_PZZDt4FMG5ROVen3bSKJ7zQvF_V5cNRCmn0pfhGzKCIS0OcNcq6WlSXmUPluqY"/>
</form>
```
{% endcode %}

</details>

The initial request with the order data coming to your `paymentUrl` is encrypted with an AES-128 mechanism in GCM mode (**aes-128-gcm**). \
\
The key for decoding is the first 16 characters of your app’s unique and unchangeable `client_secret`. Get your `clent_secret` value from the [app dashboard page](https://my.ecwid.com/#develop-apps).&#x20;

After decoding, you can access order details in the `enc_data` JSON object.

Request decoding examples in PHP and NodeJS:

{% tabs %}
{% tab title="PHP" %}
```php
<?php
function getEcwidPayload($app_secret_key, $data) {
  // Get the encryption key (16 first bytes of the app's client_secret key)
  $encryption_key = substr($app_secret_key, 0, 16);

  // Decrypt payload
  $json_data = aes_128_decrypt($encryption_key, $data);

  // Decode json
  $json_decoded = json_decode($json_data, true);
  return $json_decoded;
}

function aes_128_decrypt($key, $data) {
  // Ecwid sends data in url-safe base64. Convert the raw data to the original base64 first
  $base64_original = str_replace(array('-', '_'), array('+', '/'), $data);

  // Get binary data
  $decoded = base64_decode($base64_original);

  echo "<div>decoded: $decoded</div>";

  // Initialization vector is the first 16 bytes of the received data
  $iv = substr($decoded, 0, 16);

  // Tag is the last 16 bytes of the received data
  $tag = substr($decoded, -16);

  // The payload itself is the rest of the received data
  $payload = substr($decoded, 16, -16);

  // Decrypt raw binary payload
  $json = openssl_decrypt($payload, "aes-128-gcm", $key, true, $iv, $tag);

  return $json;
}

// Get payload from the POST and process it
$ecwid_payload = $_POST['enc_data'];
$client_secret = "QKJkxLNKSu22POC5UzxTBMoUQolefDPs"; // App's client_secret

// The resulting JSON array will be in $result variable
$result = getEcwidPayload($client_secret, $ecwid_payload);
?>
```
{% endtab %}

{% tab title="NodeJS" %}
```n4js
var crypto = require("crypto");
var EncryptionHelper = (function () {
    function decryptText(cipher_alg, key, text, encoding) {
        var bText = Buffer.from(text, encoding);
        var iv = bText.slice(0, 16);
        var tag = bText.slice(-16)
        var payload = bText.slice(16, -16);
        var decipher = crypto.createDecipheriv(cipher_alg, key, iv);
        decipher.setAuthTag(tag);
        return Buffer.concat([
          decipher.update(payload, encoding),
          decipher.final()
        ]);
    }
    return {
        decryptText: decryptText
    };
})();
module.exports = EncryptionHelper;

let client_secret = 'QKJkxLNKSu22POC5UzxTBMoUQolefDPs'; // App's client_secret
  let data = req.body.enc_data;
  let encryption_key = client_secret.substr(0, 16);
  var originalBase64 = data.replace(/-/g, "+").replace(/_/g, "/");
  var decrypted = EncryptionHelper.decryptText("aes-128-gcm", encryption_key, originalBase64, "base64");
  var payloadObject = JSON.parse(decrypted);
```
{% endtab %}
{% endtabs %}

If you are using C# language, create additional padding to make the payload a multiple of 4:

```csharp
base64 = base64.PadRight(base64.Length + (4 - (base64.Length % 4)), '=');
```

Quickstart with the [payment integration template](../quickstart-to-developing-a-payment-app.md).&#x20;

If you want to use Web Cryptography API / SublteCrypto for decoding payment requests, check out the code example created by our community developers (may be partially outdated due to encryption mechanism changes): [https://gist.github.com/manuelfdo/94a14c0314b07e311f07b240921eab86](https://gist.github.com/manuelfdo/94a14c0314b07e311f07b240921eab86).

