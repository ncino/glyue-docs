# Mask

## Parameters

### integration

<mark style="color:blue;">`relationship`</mark> - <mark style="color:red;">`required`</mark>

Integration to apply the mask to.

### servicerequest

<mark style="color:blue;">`relationship`</mark> - <mark style="color:orange;">`optional`</mark>

Service request to apply the mask to. Only used for types of `Response`.

### sequence

<mark style="color:yellow;">`integer`</mark> - <mark style="color:red;">`required`</mark>

Order in which to apply the masks for a given input or response.

### type

<mark style="color:yellow;">`string`</mark> - <mark style="color:red;">`required`</mark>

\[`Input`, `Response`] - Specifies which inputs to mask. If type is `Response`, the service request field must be specified.

### field

<mark style="color:yellow;">`code`</mark> - <mark style="color:orange;">`optional`</mark>

Field to apply mask to. Field has the current context of the associated input, response or the current iterable provided by `apply_to_each`.

Fields can either use dot-notation or bracket notation to access nested elements

```py
#dot notation
a.n
#bracket notation
["a"]
```

### apply\_if

<mark style="color:yellow;">`boolean`</mark> - <mark style="color:orange;">`optional`</mark>

`code` field that requires a boolean output.

### apply\_to\_each

<mark style="color:yellow;">`iterable`</mark> - <mark style="color:orange;">`optional`</mark>

`code` field that should specify an iterable. `apply_to_each` resolves prior to apply\_if. Once an `apply_to_each` is applied, the `field` now has the context of the iterable.

### abort\_if\_not\_applied\_x\_times

<mark style="color:yellow;">`integer`</mark> - <mark style="color:red;">`required`</mark>

Forces integration to abort if the mask is not applied at least this many times. If set to 0, this field will be not be applied. By default this is set to zero.

### notes

Documentation field.

### Usage

Masked Parameters hide the incoming response data or input data to prevent sensitive information from being displayed within internal processes.

| SSN Masking              |
| ------------------------ |
| 123-45-6789 -> XXXXXXXXX |

### Example:

Input Payload

```json
{
    "people": [
        {
            "name": "Person 1",
            "age": 42,
            "private_info": "mask this"
        },
        {
            "name": "Person 2",
            "age": 22,
            "private_info": "mask this",
            "key:12": {
                "special": true
            }
        }
    ],
    "top:level:colon": true
}
```

The following masks the `top:level:colon`, `private_info`, and special fields `special` , in that respecitive order.

<table><thead><tr><th width="189">Field</th><th width="184">abort_if_not_applied</th><th width="84">type</th><th width="147">apply_to_each</th><th>apply_if</th></tr></thead><tbody><tr><td>["top:level:colon"]</td><td>1</td><td>INPUT</td><td></td><td></td></tr><tr><td>private_info</td><td>2</td><td>INPUT</td><td>people</td><td></td></tr><tr><td>["key:12"].special</td><td>1</td><td>INPUT</td><td>people</td><td>["key:12"]</td></tr></tbody></table>

Example exported JSON for the above masks:

{% file src="../../.gitbook/assets/mask_sample_integration.json" %}
