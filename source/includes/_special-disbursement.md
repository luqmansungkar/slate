# Special Disbursement

Special Disbursement is a type of disbursement for a company operating as a Money Transfer Company (Perusahaan Transfer Dana). The difference with common disbursement, is in this transaction you must provide your senders personal information as part of the Know Your Customer (KYC) process mandated by Bank Indonesia.

## Create Special Disbursement

```http
POST /special-disbursement HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: Basic [your encoded big flip secret key]
```

<aside class="notice">We strongly suggest you to use idempotency key header to prevent accidentally created the same disbursement more than once. Please see more detail on <a href="#idempotent-request">Idempotent Request</a> section</aside>

### Request

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
    "recipient_city" => "391",
    "sender_country" => 100252,
    "sender_place_of_birth" => 391,
    "sender_date_of_birth" => "1992-01-01",
    "sender_identity_type" => "nat_id",
    "sender_name" => "paijo",
    "sender_address" => "taman bakokekok di jalan bakokekok 15 no.2 - 230",
    "sender_identity_number" => "123456789",
    "sender_job" => "private_employee",
    "direction" => "DOMESTIC_SPECIAL_TRANSFER"
];

curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($payloads));

curl_setopt($ch, CURLOPT_HTTPHEADER, array(
  "Content-Type: application/x-www-form-urlencoded"
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
    -d account_number="0437051936" \
    -d bank_code="bni" \
    -d amount="10000" \
    -d remark="testing" \
    -d recipient_city="391" \
    -d sender_country="100252" \
    -d sender_place_of_birth=391,
    -d sender_date_of_birth="1992-01-01" \
    -d sender_identity_type="nat_id" \
    -d sender_name="paijo" \
    -d sender_address="taman bakokekok di jalan bakokekok 15 no.2 - 230" \
    -d sender_identity_number="123456789" \
    -d sender_job="private_employee" \
    -d direction="DOMESTIC_SPECIAL_TRANSFER"
```

Attribute | Description
----------|-------------
account_number | **`string (required)`** <br> The account number of the recipient
bank_code | **`string (required)`** <br> Bank code of the recipient bank. Accepted value are listed [above](#disbursement)
amount | **`integer (required)`** <br> The amount of money to be disbursed (Rp20.000.000 max)
remark | **`string (optional)`** <br> Remark to be included in the transfer made to the recipient. Usually will appear as `berita transfer` or `remark` in the transfer receipt. Max length for this attribute is **18** character
recipient_city | **`integer (optional)`** <br> City code of the recipient city. This attribute is mandatory only for `bni`, `cimb`, and `bsm`. Available value can be retrieved from [city list](#city-list)
sender_country | **`integer (required)`** <br> Country code of the sender's residence. Available value can be retrieved from [country list](#country-list)
sender_place_of_birth | **`integer (required)`** <br> City/country code of the sender's place of birth. Use city code if the sender's place of birth is in Indonesia, and country code if outside Indonesia. Available value can be retrieved from [city/country list](#city-and-country-list)
sender_date_of_birth | **`string/date (required)`** <br> Sender's date of birth with `YYYY-MM-DD` format
sender_identity_type | **`string (required)`** <br> Sender's ID type. Accepted value are: <br><ul><li>`nat_id`<br>National Id Card or KTP in Indonesia</li><li>`drv_lic`<br>Driving license</li><li>`passport`</li></ul>
sender_name | **`string (required)`** <br> The name of the user of the Money Transfer Company that act as a sender
sender_address | **`string (required)`** <br> Sender's address
sender_identity_number | **`string (required)`** <br> Sender's ID number
sender_job | **`string (required)`** Sender's job. Accepted values are:<br><ul><li>`housewife`</li><li>`entrepreneur`</li><li>`private_employee`</li><li>`government_employee`</li><li>`foundation_board`<br>People who work at foundation as an employee</li><li>`indonesian_migrant_worker`<br>Also known as TKI</li><li>`others`</li></ul>
direction | **`string (required)`** The direction of the transaction. Accepted values are: <br><ul><li>`DOMESTIC_SPECIAL_TRANSFER`<br>When the sender and the recipient are both residing in Indonesia</li><li>`FOREIGN_INBOUND_SPECIAL_TRANSFER`<br>When the sender are in a foreign country, but the recipient are in Indonesia</li></ul>

### Response

```json
Status 200
Content-Type: application/json

{
    "id": 812,
    "user_id": 23,
    "amount": "10000",
    "status": "PENDING",
    "timestamp": "2017-09-06 14:29:55",
    "bank_code": "bni",
    "account_number": "0437051936",
    "recipient_name": "- FLIPTECH LENTERA INSPIRASI P",
    "sender_bank": "bri",
    "remark": "testing",
    "receipt": "",
    "time_served": "0000-00-00 00:00:00",
    "bundle_id": 0,
    "company_id": 7,
    "recipient_city": 391,
    "created_from": "API",
    "direction": "DOMESTIC_SPECIAL_TRANSFER",
    "sender": {
        "sender_name": "John Doe",
        "sender_place_of_birth": 391,
        "sender_date_of_birth": "1992-01-31",
        "sender_address": "taman bakokekok di jalan bakokekok 15 no.2 - 230",
        "sender_identity_type": "nat_id",
        "sender_identity_number": "asdas213123",
        "sender_country": 100252,
        "sender_job": "private_employee"
    },
    "fee": 1500
}
```

See detailed explanation at [get all disbursement](#get-all-disbursement) response.