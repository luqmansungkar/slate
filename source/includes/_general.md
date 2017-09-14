# General

## Get Balance

```http
GET /general/balance HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: basic [your encoded big flip secret key]
```

This endpoint will return your current account balance in Rupiah (IDR).

### Request

```php
<?php

$ch = curl_init();
$secret_key = "wwwwwwwxxxxxxxaaaaaaabbbbbbbbbcccccdddd";

curl_setopt($ch, CURLOPT_URL, "https://big.flip.id/api/v2/general/balance");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
curl_setopt($ch, CURLOPT_HEADER, FALSE);

curl_setopt($ch, CURLOPT_HTTPHEADER, array(
  "Content-Type: application/x-www-form-urlencoded"
));

curl_setopt($ch, CURLOPT_USERPWD, $secret_key.":");

$response = curl_exec($ch);
curl_close($ch);

var_dump($response);
```

```shell
curl https://big.flip.id/api/v2/general/balance \
    -u <secret_key>: 
```

### Response

```json
Status 200
Content-Type: application/json

{
    "balance": 49656053
}
```

## Get Bank Info

```http
GET /general/banks HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: basic [your encoded big flip secret key]
```

This endpoint will return list of bank codes, along with several other information. We recommend you to hit this endpoint before creating a disbursement so that you can give information like `queue` or `status` to your user or customer.

### Request

```php
<?php

$ch = curl_init();
$secret_key = "wwwwwwwxxxxxxxaaaaaaabbbbbbbbbcccccdddd";

curl_setopt($ch, CURLOPT_URL, "https://big.flip.id/api/v2/general/banks");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
curl_setopt($ch, CURLOPT_HEADER, FALSE);

curl_setopt($ch, CURLOPT_HTTPHEADER, array(
  "Content-Type: application/x-www-form-urlencoded"
));

curl_setopt($ch, CURLOPT_USERPWD, $secret_key.":");

$response = curl_exec($ch);
curl_close($ch);

var_dump($response);
```

```shell
curl https://big.flip.id/api/v2/general/banks \
    -u <secret_key>: 
```

/banks?code=**`bank_code`**

You can also provide an optional bank code to filter the result to a specific bank only

### Response

```json
Status 200
Content-Type: application/json

[
    {
        "code": "mandiri",
        "name": "Mandiri",
        "fee": 3500,
        "queue": 8,
        "status": "DISTURBED"
    },
    {
        "code": "bri",
        "name": "BRI",
        "fee": 1000,
        "queue": 39,
        "status": "OPERATIONAL"
    },
    {
        "code": "bni",
        "name": "BNI",
        "fee": 3500,
        "queue": 57,
        "status": "OPERATIONAL"
    },
    {
        "code": "bca",
        "name": "BCA",
        "fee": 3500,
        "queue": 8,
        "status": "OPERATIONAL"
    },
    {
        "code": "bsm",
        "name": "Bank Syariah Mandiri",
        "fee": 3500,
        "queue": 2,
        "status": "HEAVILY_DISTURBED"
    },
    {
        "code": "cimb",
        "name": "CIMB Niaga",
        "fee": 3500,
        "queue": 3,
        "status": "OPERATIONAL"
    },
    {
        "code": "muamalat",
        "name": "Muamalat",
        "fee": 3500,
        "queue": 1,
        "status": "OPERATIONAL"
    }
]
```

> Filtered result:

```json
Status 200
Content-Type: application/json

[
    {
        "bank_code": "bca",
        "name": "BCA",
        "fee": 3500,
        "queue": 8,
        "status": "OPERATIONAL"
    }
]
```

Attribute | Description
----------|------------
bank_code | Flip's bank code. `bni` is the code for both BNI and BNI Syariah, and `cimb` is the code for both CIMB Niaga and CIMB Niaga Syariah
name | The name of the bank as we usually say it in Indonesian
fee | The fee that you'll be charged if you send money to this bank
queue | Current queue for related bank. The longer/higher the queue number, the longer the transaction will be finished.
status | The status of the disbursement process in related bank. Possible values are: <br><ul><li>`OPERATIONAL`<br>Banks are operational, disbursement will be processed as soon as possible</li><li>`DISTURBED`<br>Banks are slow or have another problem. Disbursement will still be processed, but in slower pace and might be delayed</li><li>`HEAVILY_DISTURBED`<br>Banks are having an error, offline, or another problem that result in a nearly unusable system. Disbursement to this bank can not be processed in a short time, and maybe won't be processed in the same day. You can ask for a refund if this happen.</li></ul>

## Is Operational

```http
GET /general/operational HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: basic [your encoded big flip secret key]
```

This endpoint will return information whether Flip currently is operational or not. Our operational hour is **09.00-19.00 (WIB/GMT+7) on Monday-Friday**, and **09.00-14.00 (WIB/GMT+7) on Saturday**. Currently we are not operational on Sunday. All transactions made outside those hour will be processed on the next operational hour.

### Request

```php
<?php

$ch = curl_init();
$secret_key = "wwwwwwwxxxxxxxaaaaaaabbbbbbbbbcccccdddd";

curl_setopt($ch, CURLOPT_URL, "https://big.flip.id/api/v2/general/operational");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
curl_setopt($ch, CURLOPT_HEADER, FALSE);

curl_setopt($ch, CURLOPT_HTTPHEADER, array(
  "Content-Type: application/x-www-form-urlencoded"
));

curl_setopt($ch, CURLOPT_USERPWD, $secret_key.":");

$response = curl_exec($ch);
curl_close($ch);

var_dump($response);
```

```shell
curl https://big.flip.id/api/v2/general/operational \
    -u <secret_key>: 
```

### Response

```json
Status 200
Content-Type: application/json

{
    "operational": true
}
```

## Is Maintenance

```http
GET /general/maintenance HTTP/1.1
Content-Type: application/x-www-form-urlencoded
```

This endpoint will return information whether Flip currently is on maintenance or not. When Flip is on maintenance, you can't do any request except to this endpoint. Any request to other endpoint will return `401` status code

### Request

```php
<?php

$ch = curl_init();
$secret_key = "wwwwwwwxxxxxxxaaaaaaabbbbbbbbbcccccdddd";

curl_setopt($ch, CURLOPT_URL, "https://big.flip.id/api/v2/general/maintenance");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
curl_setopt($ch, CURLOPT_HEADER, FALSE);

curl_setopt($ch, CURLOPT_HTTPHEADER, array(
  "Content-Type: application/x-www-form-urlencoded"
));

curl_setopt($ch, CURLOPT_USERPWD, $secret_key.":");

$response = curl_exec($ch);
curl_close($ch);

var_dump($response);
```

```shell
curl https://big.flip.id/api/v2/general/maintenance \
    -u <secret_key>: 
```

### Response

```json
Status 200
Content-Type: application/json

{
    "maintenance": false
}
```