# Disbursement

Valid bank code are:

* `bca`
* `bri`
* `bni`
* `mandiri`
* `bsm`
* `muamalat`
* `cimb`

## Bank Account Inquiry

```http
POST /disbursement/bank-account-inquiry HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Authorization: basic [your encoded big flip secret key]
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

curl_setopt($curl, CURLOPT_USERPWD, $secret_key.":");

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
account_number | `string (required)` <br> The account number of the bank account
bank_code | `string (required)` <br> Bank code of the account. Accepted value are listed above

### Response

> <h3>Response example<hr></h3>

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
    "account_holder": "PT Fliptech Lentera IP",
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
status | Possible values are <br> <ul><li>`PENDING`<br>Inquiry still in process</li><li>`SUCCESS`<br>Inquiry process is complete and bank account number is valid</li><li>`INVALID_ACCOUNT_NUMBER`<br>Inquiry process is complete but the account number is invalid or maybe a virtual account number</li><li>`SUSPECTED_ACCOUNT`<br>Bank account have been suspected on doing fraud</li><li>`BLACK_LISTED`<br>Bank account have been confirmed on doing a fraud and therefore is blacklisted</li></ul>

### Callback

> <h3>Callback handler example<hr></h3>

```php
<?php
$data = isset($_POST['data']) ? $_POST['data'] : null;
$token = isset($_POST['token']) ? $_POST['token'] : null;
if($token === 'the_token_you_get_from_big_flip_dashboard'){
	$decoded_data = json_decode($data);
	print_r($decoded_data);
	//will print: 
	/**
    * {
    *     "bank_code": "bca",
    *     "account_number": "5465327020",
    *     "account_holder": "PT Fliptech Lentera IP",
    *     "status": "SUCCESS"
    * }
	*/
}
```

When our system have complete the inquiry process, we will hit the URL you've provided in your <a href="https://big.flip.id/api-info" target="_blank">Big Flip dashboard</a>. The callback process will only done once, so you have to make sure that your callback URL always in good condition. 

We will hit your URL using POST request with content type `application/x-www-form-urlencoded` and payload as described below:

Attribute | Description
----------|-------------
data | `string`<br>JSON array string with content exactly the same as the response above
token | `string`<br>Validation token to ensure that the callback is coming from our server. You can get your token in your Big Flip dashboard.

Example code of how to receive the callback are shown on the right side.