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

## Get Bank Code

```http
GET /general/banks HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: basic [your encoded big flip secret key]
```

This endpoint will return list of bank codes, along with its Indonesian name, and the fees that'll be charged if you send money to each bank.

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

### Response

```json
Status 200
Content-Type: application/json

[
    {
        "code": "mandiri",
        "nama": "Mandiri",
        "fee": 3500
    },
    {
        "code": "bri",
        "nama": "BRI",
        "fee": 1000
    },
    {
        "code": "bni",
        "nama": "BNI",
        "fee": 3500
    },
    {
        "code": "bca",
        "nama": "BCA",
        "fee": 3500
    },
    {
        "code": "bsm",
        "nama": "Bank Syariah Mandiri",
        "fee": 3500
    },
    {
        "code": "cimb",
        "nama": "CIMB Niaga",
        "fee": 3500
    },
    {
        "code": "muamalat",
        "nama": "Muamalat",
        "fee": 3500
    }
]
```

## Is Operational

```http
GET /general/operational HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: basic [your encoded big flip secret key]
```

This endpoint will return is currently Flip operational or not. Our operational hour is **09.00-19.00 (WIB/GMT+7) on Monday-Friday**, and **09.00-14.00 (WIB/GMT+7) on Saturday**. Currently we are not operational on Sunday. All transactions made outside those hour will be processed on the next operational hour.

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

This endpoint will return is Flip currently on maintenance or not. When Flip is on maintenance, you can't do any request except to this endpoint. Any request to other endpoint will return `401` status code

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
    "balance": 49656053
}
```