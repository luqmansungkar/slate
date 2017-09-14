# Disbursement

Valid bank code are:

* `bca` - Bank Central Asia
* `bri` - Bank Rakyat Indonesia
* `bni` - Bank Negara Indonesia & Bank Negara Indonesia Syariah
* `mandiri` - Bank Mandiri
* `bsm` - Bank Syariah Mandiri
* `muamalat` - Bank Muamalat
* `cimb` - CIMB Niaga & CIMB Niaga Syariah

You can also get this list of bank code along with another information in [get bank code](#get-bank-code)

## Create Disbursement

```http
POST /disbursement HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: Basic [your encoded big flip secret key]
```

Use this endpoint to create a common disbursement transaction. For company operating as a Money Transfer Company (Perusahaan Transfer Dana), or anything related to that, please use [Create Special Disbursement](#create-special-disbursement) endpoint instead.

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
    "recipient_city" => "391"
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
    -d recipient_city="391"
```

Attribute | Description
----------|-------------
account_number | **`string (required)`** <br> The account number of the recipient
bank_code | **`string (required)`** <br> Bank code of the recipient bank. Accepted value are listed [above](#disbursement)
amount | **`integer (required)`** <br> The amount of money to be disbursed (Rp20.000.000 max)
remark | **`string (required)`** <br> Remark to be included in the transfer made to the recipient. Usually will appear as `berita transfer` or `remark` in the transfer receipt. Max length for this attribute is **18** character
recipient_city | **`integer (optional)`** <br> City code of the recipient city. This attribute is mandatory only for `bni`, `cimb`, and `bsm`. Available value can be retrieved from [city list](#city-list)

### Response

```json
Status 200
Content-Type: application/json

{
    "id": 790,
    "user_id": 23,
    "amount": 10000,
    "status": "PENDING",
    "timestamp": "2017-08-28 14:32:47",
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
    "direction": "DOMESTIC_TRANSFER",
    "sender": null,
    "fee": 1500
}
```

See detailed explanation at [get all disbursement](#get-all-disbursement) response.

## Get All Disbursement

```http
GET /disbursement HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: Basic [your encoded big flip secret key]
```

Use this endpoint to get the list of transaction you've made. By default, the result will be paginated by 20. You can change the pagination, filter, and sort the result by changing the url parameter.


### Request

```php
<?php

$ch = curl_init();
$secret_key = "wwwwwwwxxxxxxxaaaaaaabbbbbbbbbcccccdddd";

curl_setopt($ch, CURLOPT_URL, "https://big.flip.id/api/v2/disbursement?pagination=pagination&page=page&sort=sort&atribut=value");
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
curl https://big.flip.id/api/v2/disbursement?pagination=pagination&page=page&sort=sort&atribut=value \
    -u <secret_key>: 
```

Format of the url parameter:

/disbursement?pagination=<b>`pagination`</b>&page=<b>`page`</b>&sort=<b>`sort`</b>&<b>`attribute`</b>=<b>`value`</b>

param | value
------|-------
pagination | **`integer (optional)`** <br> The pagination of the result. Default value is `20`
page | **`integer (optional)`** <br> The page number of the result to be viewed.
sort | **`string (optional)`** <br> Sort the result by the attribute. Use the attribute name (e.g `sort=id`) to sort in <b>ascending</b> order or dash+attribute name (e.g `sort=-id`) to sort in <b>descending</b> order.

You can also filter the result by changing **`attribute`** with the attribute to be filtered and **`value`** with the filter value. You can filter more than one attribute by appending another attribute filter to the url. Attribute that can be filtered are:

* `id` - exact comparison
* `amount` - exact comparison
* `status` - exact comparison
* `timestamp` - "like" comparison
* `bank_code` - "like" comparison
* `account_number` - "like" comparison
* `recipient_name` - "like" comparison
* `remark` - "like" comparison
* `time_served` - "like" comparison
* `created_from` - "like" comparison
* `direction` - exact comparison

Example:

<a href="#">/disbursement?pagination=10&page=5&sort=-id&status=PENDING&bank_code=bni</a>

### Response

```json
Status 200
Content-Type: application/json

{
    "total_data": 85,
    "data_per_page": 20,
    "total_page": 5,
    "page": 1,
    "data": [
        {
            "id": 790,
            "user_id": 23,
            "amount": 10000,
            "status": "PENDING",
            "timestamp": "2017-08-28 14:32:47",
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
            "direction": "DOMESTIC_TRANSFER",
            "sender": null,
            "fee": 1500
        },
        {
            "id": 789,
            "user_id": 23,
            "amount": 10000,
            "status": "DONE",
            "timestamp": "2017-08-24 21:21:23",
            "bank_code": "bni",
            "account_number": "0437051936",
            "recipient_name": "- FLIPTECH LENTERA INSPIRASI P",
            "sender_bank": "bri",
            "remark": "test remark",
            "receipt": "https://storage.biznetgiocloud.com/v1/AUTH_GIOOST443831/bukti_transfer/123993_2017-08-04%202017:07:26.jpg",
            "time_served": "2017-08-25 09:11:35",
            "bundle_id": 0,
            "company_id": 7,
            "recipient_city": 391,
            "created_from": "API",
            "direction": "FOREIGN_INBOUND_SPECIAL_TRANSFER",
            "sender": {
                "sender_name": "John Doe",
                "place_of_birth": 391,
                "date_of_birth": "1992-01-01",
                "address": "taman bakokekok di jalan bakokekok 15 no.2 - 230",
                "sender_identity_type": "nat_id",
                "sender_identity_number": "asdas213123",
                "sender_country": 100252,
                "job": "babu"
            },
            "fee": 1000
        }
    ]
}
```

Attribute | Description
----------|-------------
total_data | Total data returned in all page
data_per_page | Total data returned in current page
total_page | Total/max page available
page | Current page
data | Array of <b>disbursement object</b>.

#### Disbursement Object

Attribute | Description
----------|-------------
id | Flip's transaction id
user_id | Your account user id in our system
amount | The amount of money to be disbursed in IDR
status | Transaction status. Possible values are: <br> <ul><li>`PENDING`<br>Disbursement is still in process</li><li>`CANCELLED`<br>The transaction is cancelled and the amount of the transaction plus the transaction fee will be credited to your balance. This will happen if the transfer process are failed for reason such as inactive recipient account, wrong account number, etc</li><li>`DONE`<br>Disbursement process is finished and the money have been sent to the recipient</li></ul>
timestamp | The time when the disbursement request created. Time will be in GMT+7 with `yyyy-mm-dd hh:mm:ss` format
bank_code | Bank code of the recipient bank
account_number | The account number of the recipient
recipient_name | The name of the recipient account holder. If the account number haven't cached by Flip yet, this attribute will show <b>`-`</b> (dash) instead
sender_bank | The default sender bank in your account
remark | Remark to be included in the transfer made to the recipient
receipt | Url of the transfer receipt. The receipt will be a screenshot taken from the internet banking interface of each bank. This attribute will only show the url when the status is `DONE`
time_served | The time when the transaction is finished. Will only show valid value when the status is `DONE`
bundle_id | The bundle id of the transaction made from Big Flip Dashboard (csv upload or manual input). For the transaction created from API, the value will always be `0`
company_id | Your Big Flip account user id in our system
recipient_city | City code of the recipient city
created_from | The channel of which the transaction was created. Possible values are: <br><ul><li>`API`</li><li>`DASHBOARD`</li></ul>
direction | The direction of the transaction. Possible values are: <br><ul><li>`DOMESTIC_TRANSFER`<br>Common Disbursement from Indonesia to Indonesian recipient</li><li>`DOMESTIC_SPECIAL_TRANSFER`<br>Special disbursement from the user of  a Money Transfer Company residing in Indonesia to Indonesian recipient</li><li>`FOREIGN_INBOUND_SPECIAL_TRANSFER`<br>Special disbursement from the user of a Money Transfer Company residing in a foreign country to Indonesian recipient</li></ul>
sender | Possible values are `null` if the transaction is a common disbursement, and <b>sender object</b> if the transaction is a special disbursment.
fee | The fee of the transaction

#### Sender Object

Attribute | Description
----------|-------------
sender_name | The name of the user of the Money Transfer Company that act as a sender
place_of_birth | City/country code of the Sender's place of birth
date_of_birth | Sender's date of birth
address | Sender's address
sender_identity_type | Sender's identity type. Possible value are: <br><ul><li>`nat_id`</li><li>`drv_lic`</li><li>`passport`</li></ul>
sender_identity_number | Sender's identity number
sender_country | Country code of the Sender's country
job | Sender's job. Possible values are:<br><ul><li>`housewife`</li><li>`entrepreneur`</li><li>`private_employee`</li><li>`government_employee`</li><li>`foundation_board`</li><li>`indonesian_migrant_worker`</li><li>`others`</li></ul>

## Get Disbursement

```http
GET /disbursement/{transaction_id} HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: Basic [your encoded big flip secret key]
```

Use this endpoint to get one specific transaction specified by `transaction_id` in the request url.

### Request

```php
<?php

$ch = curl_init();
$secret_key = "wwwwwwwxxxxxxxaaaaaaabbbbbbbbbcccccdddd";

curl_setopt($ch, CURLOPT_URL, "https://big.flip.id/api/v2/disbursement/123");
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
curl https://big.flip.id/api/v2/disbursement/123 \
    -u <secret_key>: 
```

### Response

```json
Status 200
Content-Type: application/json

{
    "id": 790,
    "user_id": 23,
    "amount": 10000,
    "status": "PENDING",
    "timestamp": "2017-08-28 14:32:47",
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
    "direction": "DOMESTIC_TRANSFER",
    "sender": null,
    "fee": 1500
},
```

See detailed explanation at [get all disbursement](#get-all-disbursement) response

## Disbursement Queue

```http
GET /disbursement/{transaction_id}/queue HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: Basic [your encoded big flip secret key]
```

Each disbursement transaction will queued to be sent to the recipient. You can view the queue number for each transanction in this endpoint.


### Request

```php
<?php

$ch = curl_init();
$secret_key = "wwwwwwwxxxxxxxaaaaaaabbbbbbbbbcccccdddd";

curl_setopt($ch, CURLOPT_URL, "https://big.flip.id/api/v2/disbursement/123/queue");
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
curl https://big.flip.id/api/v2/disbursement/123/queue \
    -u <secret_key>: 
```

`/disbursement/{transaction_id}/queue`

Replace `{transaction_id}` with the desired transaction id.


### Response

```json
Status 200
Content-Type: application/json

{
    "queue": 24
}
```

## Disbursement Status

```http
GET /disbursement/status HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: Basic [your encoded big flip secret key]
```

This endpoint will return the status of all banks available in flip.id along with it's current queue.

### Request

```php
<?php

$ch = curl_init();
$secret_key = "wwwwwwwxxxxxxxaaaaaaabbbbbbbbbcccccdddd";

curl_setopt($ch, CURLOPT_URL, "https://big.flip.id/api/v2/disbursement/status");
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
curl https://big.flip.id/api/v2/disbursement/status \
    -u <secret_key>: 
```

### Response

```json
Status 200
Content-Type: application/json

[
    {
        "bank_code": "mandiri",
        "queue": 8,
        "status": "DISTURBED"
    },
    {
        "bank_code": "bri",
        "queue": 26,
        "status": "OPERATIONAL"
    },
    {
        "bank_code": "bni",
        "queue": 12,
        "status": "OPERATIONAL"
    },
    {
        "bank_code": "bca",
        "queue": 7,
        "status": "OPERATIONAL"
    },
    {
        "bank_code": "bsm",
        "queue": 2,
        "status": "HEAVILY_DISTURBED"
    },
    {
        "bank_code": "cimb",
        "queue": 3,
        "status": "OPERATIONAL"
    },
    {
        "bank_code": "muamalat",
        "queue": 1,
        "status": "OPERATIONAL"
    }
]
```

Attribute | Description
----------|------------
bank_code | Flip's bank code
queue | Current queue for related bank. The longer/higher the queue number, the longer the transaction will be finished.
status | The status of the disbursement process in related bank. Possible values are: <br><ul><li>`OPERATIONAL`<br>Banks are operational, disbursement will be processed as soon as possible</li><li>`DISTURBED`<br>Banks are slow or have another problem. Disbursement will still be processed, but in slower pace and might be delayed</li><li>`HEAVILY_DISTURBED`<br>Banks are having an error, offline, or another problem that result in a nearly unusable system. Disbursement to this bank can not be processed in a short time, and maybe won't be processed in the same day. You can ask for a refund if this happen.</li></ul>

## City List

```http
GET /disbursement/city-list HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: Basic [your encoded big flip secret key]
```

This endpoint will return the list of available city code along with it's name. The city name will be in Indonesian.


### Request

```php
<?php

$ch = curl_init();
$secret_key = "wwwwwwwxxxxxxxaaaaaaabbbbbbbbbcccccdddd";

curl_setopt($ch, CURLOPT_URL, "https://big.flip.id/api/v2/disbursement/city-list");
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
curl https://big.flip.id/api/v2/disbursement/city-list \
    -u <secret_key>: 
```

### Response

```json
Status 200
Content-Type: application/json

{
    "102": "Kab. Bekasi",
    "103": "Kab. Purwakarta",
    "106": "Kab. Karawang",
    "108": "Kab. Bogor",
    "109": "Kab. Sukabumi",
    "110": "Kab. Cianjur",
    "111": "Kab. Bandung",
    "112": "Kab. Sumedang",
    "113": "Kab. Tasikmalaya",
    "114": "Kab. Garut",
    "115": "Kab. Ciamis",
    "116": "Kab. Cirebon",
    "117": "Kab. Kuningan",
    "118": "Kab. Indramayu",
    "119": "Kab. Majalengka",
    ...
}
```

## Country List

```http
GET /disbursement/country-list HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: Basic [your encoded big flip secret key]
```

This endpoint will return the list of available country code along with it's name. The country name will be in Indonesian.


### Request

```php
<?php

$ch = curl_init();
$secret_key = "wwwwwwwxxxxxxxaaaaaaabbbbbbbbbcccccdddd";

curl_setopt($ch, CURLOPT_URL, "https://big.flip.id/api/v2/disbursement/country-list");
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
curl https://big.flip.id/api/v2/disbursement/country-list \
    -u <secret_key>: 
```

### Response

```json
Status 200
Content-Type: application/json

{
    "100000": "Afganistan",
    "100002": "Albania",
    "100003": "Aljazair",
    "100004": "Samoa Amerika",
    "100005": "Andorra",
    "100006": "Angola",
    "100007": "Anguilla",
    "100008": "Antartika",
    "100009": "Antigua dan Barbuda",
    "100010": "Argentina",
    "100011": "Armenia",
    ...
}
```

## City and Country List

```http
GET /disbursement/city-country-list HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: Basic [your encoded big flip secret key]
```

This is just a combination from city and country list.


### Request

```php
<?php

$ch = curl_init();
$secret_key = "wwwwwwwxxxxxxxaaaaaaabbbbbbbbbcccccdddd";

curl_setopt($ch, CURLOPT_URL, "https://big.flip.id/api/v2/disbursement/city-country-list");
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
curl https://big.flip.id/api/v2/disbursement/city-country-list \
    -u <secret_key>: 
```

### Response

```json
Status 200
Content-Type: application/json

{
    "102": "Kab. Bekasi",
    "103": "Kab. Purwakarta",
    "106": "Kab. Karawang",
    "108": "Kab. Bogor",
    "109": "Kab. Sukabumi",
    "110": "Kab. Cianjur",
    "111": "Kab. Bandung",
    "112": "Kab. Sumedang",
    "113": "Kab. Tasikmalaya",
    "114": "Kab. Garut",
    "115": "Kab. Ciamis",
    "116": "Kab. Cirebon",
    "117": "Kab. Kuningan",
    "118": "Kab. Indramayu",
    "119": "Kab. Majalengka",
    ...
}
```


## Bank Account Inquiry

```http
POST /disbursement/bank-account-inquiry HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: Basic [your encoded big flip secret key]
```


You can use this endpoint to get the bank account holder name. For now, it still take us a few seconds to do the inquiry. The result will be returned as a callback if we haven't cached it yet. If it have been cached, you will get the result instantly. Be sure to set up your callback inquiry entry in your <a href="https://big.flip.id/api-info" target="_blank">Big Flip dashboard</a>.

### Request

```php
<?php

$ch = curl_init();
$secret_key = "wwwwwwwxxxxxxxaaaaaaabbbbbbbbbcccccdddd";

curl_setopt($ch, CURLOPT_URL, "https://big.flip.id/api/v2/disbursement/bank-account-inquiry");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
curl_setopt($ch, CURLOPT_HEADER, FALSE);

curl_setopt($ch, CURLOPT_POST, TRUE);

curl_setopt($ch, CURLOPT_POSTFIELDS, "account_number=5465327020&bank_code=bca");

curl_setopt($ch, CURLOPT_HTTPHEADER, array(
  "Content-Type: application/x-www-form-urlencoded"
));

curl_setopt($ch, CURLOPT_USERPWD, $secret_key.":");

$response = curl_exec($ch);
curl_close($ch);

var_dump($response);
```

```shell
curl https://big.flip.id/api/v2/disbursement/bank-account-inquiry \
    -X POST \
    -u <secret_key>: \
    -d account_number="5465327020" \
    -d bank_code="bca"
```

Attribute | Description
----------|-------------
account_number | **`string (required)`** <br> The account number of the bank account
bank_code | **`string (required)`** <br> Bank code of the account. Accepted value are listed [above](#disbursement)

### Response

> Example of cached response

```json
Status 200
Content-Type: application/json

{
    "bank_code": "bca",
    "account_number": "5465327020",
    "account_holder": "PT Fliptech Lentera IP",
    "status": "SUCCESS"
}
```

> Example of uncached response

```json
Status 200
Content-Type: application/json

{
    "bank_code": "bca",
    "account_number": "5465327020",
    "account_holder": "",
    "status": "PENDING"
}
```

> Example of invalid account

```json
Status 200
Content-Type: application/json

{
    "bank_code": "bca",
    "account_number": "1232123123212",
    "account_holder": "",
    "status": "INVALID_ACCOUNT_NUMBER"
}
```

Attribute | Description
----------|-------------
bank_code | Bank code of the account
account_number | Account number of the bank account
account_holder | Name of the bank account holder
status | Possible values are <br> <ul><li>`PENDING`<br>Inquiry still in process</li><li>`SUCCESS`<br>Inquiry process is complete and bank account number is valid</li><li>`INVALID_ACCOUNT_NUMBER`<br>Inquiry process is complete but the account number is invalid or maybe a virtual account number</li><li>`SUSPECTED_ACCOUNT`<br>Bank account have been suspected on doing fraud. You still can do a disbursement to this account.</li><li>`BLACK_LISTED`<br>Bank account have been confirmed on doing a fraud and therefore is blacklisted. You can't do a disbursment to this account.</li></ul>

<aside class="notice"><code class="prettyprint">SUSPECTED_ACCOUNT</code> and <code class="prettyprint">BLACK_LISTED</code> are based on Flip's user report.  This might be inacurrate, and we flag an exact string of the account number reported by our users. For example, <code class="prettyprint">350000069</code> will be different with <code class="prettyprint">00350000069</code> although it may be the same account. We are still working on a better way to handle this leading zeroes issue.</aside>