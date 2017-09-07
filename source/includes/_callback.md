# Callback

```php
<?php
$data = isset($_POST['data']) ? $_POST['data'] : null;
$token = isset($_POST['token']) ? $_POST['token'] : null;
if($token === 'the_token_you_get_from_big_flip_dashboard'){
	$decoded_data = json_decode($data);
	print_r($decoded_data);
	//example of what will be printed are listed below
}
```

> Example of bank account inquiry

```json
{
    "bank_code": "bca",
    "account_number": "5465327020",
    "account_holder": "PT Fliptech Lentera IP",
    "status": "SUCCESS"
}
```

> Example of disbursement

```json
{
    "id": 790,
    "user_id": 23,
    "amount": 10000,
    "status": "DONE",
    "timestamp": "2017-08-28 14:32:47",
    "bank_code": "bni",
    "account_number": "0437051936",
    "recipient_name": "- FLIPTECH LENTERA INSPIRASI P",
    "sender_bank": "bri",
    "remark": "testing",
    "receipt": "https://storage.biznetgiocloud.com/v1/AUTH_GIOOST443831/bukti_transfer/123993_2017-08-04%202017:07:26.jpg",
    "time_served": "2017-08-28 14:42:47",
    "bundle_id": 0,
    "company_id": 7,
    "recipient_city": 391,
    "created_from": "API",
    "direction": "DOMESTIC_TRANSFER",
    "sender": null,
    "fee": 1500
}
```

> Example of special disbursement

```json
{
    "id": 812,
    "user_id": 23,
    "amount": "10000",
    "status": "DONE",
    "timestamp": "2017-09-06 14:29:55",
    "bank_code": "bni",
    "account_number": "0437051936",
    "recipient_name": "- FLIPTECH LENTERA INSPIRASI P",
    "sender_bank": "bri",
    "remark": "testing",
    "receipt": "",
    "time_served": "2017-09-06 14:39:55",
    "bundle_id": 0,
    "company_id": 7,
    "recipient_city": 391,
    "created_from": "API",
    "direction": "DOMESTIC_SPECIAL_TRANSFER",
    "sender": {
        "sender_name": "kebelet",
        "place_of_birth": 391,
        "date_of_birth": "1992-01-31",
        "address": "taman bakokekok di jalan bakokekok 15 no.2 - 230",
        "sender_identity_type": "nat_id",
        "sender_identity_number": "asdas213123",
        "sender_country": 100252,
        "job": "private_employee"
    },
    "fee": 1500
}
```

When your transaction status changed to `DONE` or `WRONG_ACCOUNT_NUMBER`, or when our system have complete the bank account inquiry process, we will hit the URL you've provided in your <a href="https://big.flip.id/api-info" target="_blank">Big Flip dashboard</a>. The provided URL must return a `200` HTTP Status Code. If the URL return another HTTP Status Code, our system will retry the request 5 times, with 2 minute interval for disbursement callback. For bank account inquiry, we'll only do the callback once, so you have to make sure that your callback URL always in good condition.

We will hit your URL using POST request with content type `application/x-www-form-urlencoded` and payload as described below:

Attribute | Description
----------|-------------
data | `string`<br>JSON array string with content exactly the same as the response of disbursement or bank account inquiry (see example on the right side)
token | `string`<br>Validation token to ensure that the callback is coming from our server. You can get your token in your Big Flip dashboard.

Example code of how to receive the callback are shown on the right side.