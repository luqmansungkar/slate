## General Explanation

To know if the request is success or failed, you can see the `HTTP Status Code` on each response. [#](http://www.restapitutorial.com/httpstatuscodes.html)

Every request should be sent as `application/x-www-form-urlencoded` unless told differently. The body of the request must be sent as post data (e.g `attribute=value&attribute2=value2&attribute3=value3`).

**422** status code is used to indicate the mistake from your side. The common response for this status code is like on the right side:

```json
{
    "code": [outer_code],
    "errors": [
        {
            "attribute": "[attribute]",
            "code": [inner_code],
            "message": "[message]"
        }
    ]
}
```

Possible value for `outer_code` is:

* `BALANCE_INSUFFICIENT`, happen when your Flip account balance are insufficient for the current transaction (balance < (transfer amount + transfer fee)).
*  `VALIDATION_ERROR`, error related to the validation of your payload data.

Possible value for `inner_code` is all the code listed in the [Error Code section](#error-code).

## Supported Banks

Currently, we only support disbursement to these banks:

* BCA or Bank Central Asia
* BRI or Bank Rakyat Indonesia
* BNI or Bank Negara Indonesia
* BNI Syariah
* Mandiri or Bank Mandiri
* BSM or Bank Syariah Mandiri
* Muamalat or Bank Muamalat
* CIMB or CIMB Niaga
* CIMB Niaga Syariah