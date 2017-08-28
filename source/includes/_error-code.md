## Error Code

As mentioned above, validation error or response with status code `422` will contain inner code. Here is the list of the code:


Error Code | Meaning
---------- | -------
999 | Undefined error. Or maybe we just missed to give the error a proper error code. Please contact us if this happen.
1001 | The related attribute should not be empty
1002 | Value for the related attribute is considered not clean. Things that considered as not clean is html tag and `?`, `#`, `$`, and `;` character
1020 | The related attribute can only contain number
1021 | Transfer amount are less than the minimum amount (Rp10.000)
1022 | Transfer amount are more than the maximum amount (Rp20.000.000)
1024 | Max char for the related attribute exceeded
1025 | Invalid bank account number
1026 | The recipient bank account is suspected with fraud. You can't send money to this account
1027 | The related BRI bank account number is not exactly 15 character
1032 | Pagination should be a number more than 0
1033 | Invalid bank code
1035 | Insufficient Big Flip balance
1037 | Country code is not valid. Available country code is in (country code endpoint)[#ccode]
1038 | Country/city code is not valid. The difference with `1037` code is `1037` will occur if the attribute only allow country code while this code will occure if the attribute allow country or city code.
1039 | Date format is not valid according to the requested format
1040 | Date is not valid (e.g Feb 29 when not in leap year, future date, etc)
1041 | The related attribute value is out of the allowed value