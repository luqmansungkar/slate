## General Response

### Unauthorized

```json
Status 401
Content-Type: application/json

{
    "name": "Unauthorized",
    "message": "You are requesting with an invalid credential.",
    "status": 401,
}
```

This response will be sent if the value of Authorization heder or the secret key is invalid.

### Not Found

```json
Status 404
Content-Type: application/json

{
    "name": "Not Found",
    "message": "Page not found.",
    "status": 404,
}
```

You will get this response if the url you're trying to access is wrong, or if the resource you're trying to access is not exist.