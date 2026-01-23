# Validation Rule

## Parameters

### integration

<mark style="color:blue;">`relationship`</mark> - <mark style="color:red;">`required`</mark>

### servicerequest

<mark style="color:blue;">`relationship`</mark> - <mark style="color:red;">`required`</mark>

### sequence

<mark style="color:yellow;">`integer`</mark> - <mark style="color:red;">`required`</mark>

### type

<mark style="color:yellow;">`string`</mark> - <mark style="color:red;">`required`</mark>

* `INPUT`: applies on the input payload received by the integration
* `RESPONSE`: applies to payload received from adapter
* `REQUEST`: applies to payload sent to adapter

### field

<mark style="color:yellow;">`boolean`</mark> - <mark style="color:red;">`required`</mark>

`default`: True Validation rule is assesed on the boolean value returned from `field`.

### rule

<mark style="color:yellow;">`code`</mark> - <mark style="color:red;">`required`</mark>

### apply\_if

<mark style="color:yellow;">`boolean`</mark> - <mark style="color:orange;">`optional`</mark>

`default`: True. Will only apply the validation rule if these resolves to True.

### apply\_to\_each

<mark style="color:yellow;">`iterable`</mark> - <mark style="color:red;">`required`</mark>

### abort\_on\_failure

<mark style="color:yellow;">`boolean`</mark> - <mark style="color:red;">`required`</mark>

### valuemappingset

<mark style="color:blue;">`relationship`</mark> - <mark style="color:orange;">`optional`</mark>

### valuemappingset\_side

<mark style="color:yellow;">`string`</mark> - <mark style="color:orange;">`optional`</mark>

### max\_length

### message\_substitution\_name

### Validation Rule Hooks

These are code blocks that are run during processes of the validation rule. See the [Integration Life Cycle Diagram](https://github.com/AlphaPack/glyue-docs/blob/main/media/integration_life_cycle.svg) for timing of the hooks.

* pass\_hook
* fail\_hook
