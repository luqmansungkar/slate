## Environment

### Sandbox

We provide a sandbox/testing environment for you. The base URL for the sandbox is [https://sandbox.flip.id/api/v2/](https://sandbox.flip.id/api/v2/).

The sandbox environment is completely separated from the production environment, so whatever you'll do in sandbox will not affect your Big Flip account. Each sandbox account will be given Rp50.000.000 balance for you to test the API.

**The secret key and the validation token in sandbox will be different with the production environment.**

We won't process any transaction you've made in the sandbox environment. Any transaction created will be left at `PENDING` status. However, you can ask us to manually trigger the transaction to simulate `DONE` or `WRONG_ACCOUNT_NUMBER` status. You can tell us via the in browser chat feature or email me at luqman@flip.id and give us the transaction id you want to trigger.

If you doesn't have a Big Flip account yet, or just want to try the API without registering, you can use this sample secret key:

`JDJ5JDEzJGRYZ04zeGtkZkdwRy5NRm82WjM5bi5nV3NxYVByVG9YQnVPSDFaR1JZNDUxSjBHLy5yUUhp`

### Production

The production environment base url is [https://big.flip.id/api/v2/](https://big.flip.id/api/v2/). Every transaction made through this url will be processed (except if something goes wrong).

<aside class="warning">You are fully responsible to the safety of your secret key and validation token in your application. Negligence in storing those information can result in unauthorized use of your balance.<br><br>
If you suspect those information have been leaked, you can change it in your <a href="https://big.flip.id/api-info" target="_blank">Big Flip dashboard</a>.
</aside>
<aside class="notice">The changing of the secret key and/or validation token will result in unusable old key and/or token</aside>