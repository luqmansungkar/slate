## Error Code

As mentioned above, validation error or response with status code `422` will contain inner code. Here is the list of the code:


Error Code | Meaning
---------- | -------
999 | Undefined error. Or maybe we just missed to give the error a proper error code. Please contact us if this happen.
1001 | The related attribute should not be empty
1002 | Value for the related attribute is considered not clean. Things that considered as not clean is html tag and `?`, `#`, `$`, and `;` character
1020 | The related attribute can only contain number
1021 | Transfer amount are less than the minimum amount (Rp10.000)
1024 | Max char for the related attribute exceeded
1025 | Invalid bank account number
1026 | The recipient bank account is suspected with fraud. You can't send money to this account
1027 | The related BRI bank account number is not exactly 15 character
1032 | Pagination should be a number more than 0
1033 | Invalid bank code
1035 | Insufficient Big Flip balance