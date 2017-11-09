## Idempotent Request

```php
<?php

$ch = curl_init();
$secret_key = "wwwwwwwxxxxxxxaaaaaaabbbbbbbbbcccccdddd";

curl_setopt($ch, CURLOPT_URL, "https://big.flip.id/api/v2/disbursement");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
curl_setopt($ch, CURLOPT_HEADER, FALSE);

curl_setopt($ch, CURLOPT_POST, TRUE);

$payloads = [
    "account_number" => "0437051936",
    "bank_code" => "bni",
    "amount" => "10000",
    "remark" => "testing",
    "recipient_city" => "391"
];

curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($payloads));

curl_setopt($ch, CURLOPT_HTTPHEADER, array(
  "Content-Type: application/x-www-form-urlencoded",
  "idempotency-key: 8anU9saqIU798wOo"
));

curl_setopt($ch, CURLOPT_USERPWD, $secret_key.":");

$response = curl_exec($ch);
curl_close($ch);

var_dump($response);
```

```shell
curl https://big.flip.id/api/v2/disbursement \
    -X POST \
    -u <secret_key>: \
    -H "idempotency-key: 8anU9saqIU798wOo" \
    -d account_number="0437051936" \
    -d bank_code="bni" \
    -d amount="10000" \
    -d remark="testing" \
    -d recipient_city="391"
```

This version of API supports [idempotency](http://restcookbook.com/HTTP%20Methods/idempotency/), so you can safely retrying request without accidentally performing the same operation twice. If you send a [create a disbursement](#create-disbursement) and the request fails due to network error or other factor, you can safely retry the request with the same idempotency key and be sure that only a single disbursement is created.

To perform an idempotent request, you can just provide an additonal `idempotency-key` header to your request.

You can provide any unique string to be the idempotency key, such as the transaction id on your side, or another random and unique identifier from your side. The string however, will be limited to 255 char in length. 

This idempotency key will be stored forever, so the key can't be reused. We'll always send back the same response for request made with the same key.