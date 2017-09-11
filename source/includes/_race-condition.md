## Race Condition

```json
Status 500
Content-Type: application/json

{
    "code": "RACE_CONDITION",
    "errors": [
        {
            "attribute": "id",
            "code": 999,
            "message": "Race condition happened, please retry the request"
        }
    ]
}
```

Race condition will happen if there are several transfer request made at nearly the same time (only several milisecon gap). This will sometimes result in inconsistency of your balance. To prevent this from happened, our system will only accept the first transaction, and foil the others. The foiled request will get response shown on the right side.

Our system will make several attemp internally before foiling your request. But in case this is still happened, your system should retry the request if it get this response.