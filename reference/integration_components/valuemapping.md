# Value Mapping

_To learn more about when to use a Value Mapping and how to create one, see_ [_follow this how-to guide_](../../how-to-guides/how-to-create-a-value-mapping-set.md)_._

### valuemappingset

<mark style="color:blue;">`relationship`</mark> - <mark style="color:red;">`required`</mark>

Value Mapping Set that the Value Mapping belongs to.

### from\_value

<mark style="color:yellow;">`string | bool`</mark> - <mark style="color:red;">`required`</mark>

The original value of the field mapping, to identify which value mapping to use. Only supports String and Boolean values.

### to\_value

<mark style="color:yellow;">`string`</mark> - <mark style="color:red;">`required`</mark>

The transformed value that will replace the original field mapping value. Only supports String values.

### notes

<mark style="color:yellow;">`string`</mark> - <mark style="color:orange;">`optional`</mark>

Documentation field
