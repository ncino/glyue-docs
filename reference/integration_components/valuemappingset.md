# Value Mapping Set

_To learn more about when to use a Value Mapping Set and how to create one, see_ [_follow this how-to guide_](../../how-to-guides/how-to-create-a-value-mapping-set.md)_._

### integration

<mark style="color:blue;">`relationship`</mark> - <mark style="color:red;">`required`</mark>

Integration value mapping set is applied to.

### name

<mark style="color:yellow;">`string`</mark> - <mark style="color:red;">`required`</mark>

Label for the Value Mapping Set

### description

<mark style="color:yellow;">`string`</mark> - <mark style="color:orange;">`optional`</mark>

Documentation field used for value mapping description

### case\_sensitive

<mark style="color:yellow;">`boolean`</mark> - <mark style="color:red;">`required`</mark>

If `True` will respect string case. Otherwise, all input strings will be converted to lower case for the purposes of finding the corresponding value mapping. Non-string inputs are unaffected.

### use\_default\_to\_value

<mark style="color:yellow;">`boolean`</mark> - <mark style="color:red;">`required`</mark>

Flag to determine whether or not a default value should be used if no value mappings match the [field mapping value](https://github.com/AlphaPack/glyue-docs/blob/main/reference/integration_components/fieldmapping.md#value-code). If set to `False` and no value mappings match, the original value will be passed through untransformed.

### default\_to\_value (boolean | string)

<mark style="color:yellow;">`boolean | string`</mark> - <mark style="color:red;">`required`</mark>

The default value to use if no field mappings match the input value, and `use_default_to_value`is set to `True`.
