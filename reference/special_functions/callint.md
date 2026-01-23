# callint

`callint(path_name: str, payload: Any, extended_payload: Bool = False, force_run_mode: str = None) -> Any`&#x20;

Used to call an integration from within the process of the currently running integration. `callint` will return the integration output.&#x20;

| Parameters               | Description                                                                                                                                                                                                                                                                             |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `path_name`              | The integration name                                                                                                                                                                                                                                                                    |
| `payload`                | String or dict object to be used as the integration's `Input` payload.                                                                                                                                                                                                                  |
| `extended_payload=False` | See Extended Payload section below.                                                                                                                                                                                                                                                     |
| `force_run_mode=None`    | <p>Allows overriding the called integrations <code>run_async</code> field.<br><br><code>None</code>: No override (default)<br><code>sync</code>: Force <code>run_async</code> to be <code>False</code><br><code>async</code>: Force <code>run_async</code> to be <code>True</code>.</p> |

```python
payload = "input_str"
x = callint("test_integration", payload)
```

{% hint style="info" %}
Integrations triggered by `callint()`  will appear in the Run History.
{% endhint %}

### Extended Payload <a href="#extended-payload" id="extended-payload"></a>

When `extended_payload` is `False` (default), the called integration runs as if it was triggered from a `POST` request to `/integrations/execute/[integration_path_name]` with `payload` representing the body of the post request.

When `extended_payload` is `True`, the object passed to `payload`  can have the following additional keys (all optional):

* `params`
* `path`
* `headers`
* `method`
* `fullpath`

{% hint style="info" %}
Specifics on each field can be found [here](../special_variables/input.md).
{% endhint %}

This is useful for emulating the behavior of calling an integration from a [Web Services endpoint](../web-service-endpoints.md). For example,

```
payload_with_method = {
    "payload": {
        "firstName": "Sally",
        "lastName": "Smith",
    },
    "method": "POST"
}

x = callint("user", payload_with_method, extended_payload=True)
```

<br>
