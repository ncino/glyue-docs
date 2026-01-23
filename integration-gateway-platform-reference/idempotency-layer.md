# Idempotency Layer

Integration Gateway features a built-in [idempotency](https://en.wikipedia.org/wiki/Idempotence) layer for all integrations that enables callers of integrations to retry requests without concern of unintentionally duplicating the operation.

When the `Idempotency-Key` header is present, any subsequent calls from the same account with the same `Idempotency-Key` value will be treated as a duplicate request and will be returned the same response as the initial request **without re-running the integration**_._&#x20;

Resending a request with an already-used `Idempotency-Key` is most appropriate when the client does not receive a response from Integration Gateway, e.g. due to a network connectivity drop. In such situations, retrying a request with the same `Idempotency-Key` will allow the client to safely determine whether the operation actually succeeded, or if it should try again with a new `Idempotency-Key`**.**



### **Activating the Idempotency Layer**

To activate the idempotency layer, add the `Idempotency-Key` header to the request with the unique ID.&#x20;

Integration Gateway guarantees that integrations will not be re-run if they are called with:

* The same idempotency key
* From the same Integration Gateway account
* Within a 24 hour period from the original request

Instead, the status and response of the first run will be returned.

IDs may be up to 64 characters long.

{% hint style="info" %}
It is strongly recommended that a UUID generator is used to create the idempotency ID.
{% endhint %}



### Responses from the Idempotency Layer

When the idempotency layer detects a duplicate request, it will send a response that contains the same body, status, and headers that were sent in the original response. It contains no indication that the request was captured by the idempotency layer.

If a duplicate request (with the same `Idempotency-Key`) is sent before the original request finishes, the idempotency layer will response with an [HTTP 409 status](https://www.ietf.org/archive/id/draft-ietf-httpapi-idempotency-key-header-01.html#name-error-scenarios). The integration will not be re-run.&#x20;

In Integration Gateway's _Run History,_ duplicate requests are denoted by the IDEMPOTENT\_REQUEST flag within the integration run's steps.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2024-12-23 at 12.36.54 PM.png" alt="" width="453"><figcaption></figcaption></figure>



### Sample Client Code

Include the `Idempotency-Key` header in your request, setting its value to a unique key. Generate a new key for each request and retain the key for a given request until you are certain you no longer need to retry it.

```bash
curl --location 'https://glyuedev.yourdomain.sandboxbanking.com/execute/path/to/integration/here' \
--header 'Idempotency-Key: IDEMPOTENCY_KEY_HERE' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic ENCODED_BASIC_AUTH_HERE' \
--data '{
    "foo": "bar"
}'

```

