# Field Mapping

### servicerequest

<mark style="color:blue;">`relationship`</mark> - <mark style="color:red;">`required`</mark>

The service request that the field mapping belongs to.

### sequence

<mark style="color:yellow;">`integer`</mark> - <mark style="color:red;">`required`</mark>

Order in which the field mappings associated with the service request will execute. Note that this does not control the order of fields in the body of the external request — field ordering is arbitrary.

### field

<mark style="color:yellow;">`string`</mark> - <mark style="color:red;">`required`</mark>

The name of the field in the request body of the service request. Supports lazy-instantiation of intermediate fields. For example, if request body has the shape:

```json
{
    "person": {
        "address": {
            "street": "123 main street",
            "city": "Boston"
        }
    }
}
```

You can specify the `street`field using `person.address.street` directly.

### value

<mark style="color:green;">`expression`</mark> - <mark style="color:red;">`required`</mark>

The value to associate with a field. Can be a literal value, a reference to value defined elsewhere in the integration, or a code expression. If the expression does not evaluate to a value, the [retvalue variable](../special_variables/#retvalue) must be used.

### value\_type

<mark style="color:yellow;">`string`</mark> - <mark style="color:red;">`required`</mark>

Typecast to perform on non-null values. If the value is an expression, the typecast is performed on the result of the expression. Options are:

* `str` — String
* `int` — Integer
* `float` — Floating point number
* `dict` — Dictionary (also known as "Object")

Incompatible typecasting (e.g. `dict('5.0')`) will raise an error at runtime.

### valuemappingset

<mark style="color:blue;">`relationship`</mark> - <mark style="color:orange;">`optional`</mark>

The [valuemappingset](valuemappingset.md) to apply to the [#value](field-mapping.md#value "mention").

### include\_if

<mark style="color:green;">`expression`</mark> - <mark style="color:orange;">`optional`</mark>

An expression to run to determine whether the field mapping should be included in the request. Must return a boolean. Defaults to `True`if left empty.

### include\_for\_each

<mark style="color:yellow;">`iterable`</mark> - <mark style="color:orange;">`optional`</mark>

Specifies an iterable. Will produce a new field mapping entry on the request for each entry in the iterable. Use [fitem](../special_variables/#iterable-within-integrations-xx-item-and-xx-idx) and [fidx](../special_variables/#iterable-within-integrations-xx-item-and-xx-idx) to refer to values in the iterable to differentiate the field mappings. &#x20;

Resolves before the `include_if` block.

### nullable

<mark style="color:yellow;">`boolean`</mark> - <mark style="color:red;">`required`</mark>

Indicator for whether the [#value](field-mapping.md#value "mention") can be be null at runtime. If set to `false`and a null value is encountered, the integration will stop execution with an error.

### message\_substitution\_name

<mark style="color:yellow;">`string`</mark> - <mark style="color:orange;">`optional`</mark>

Replaces the sritem with specified string.

### Documentation Fields

* `target_record_type`
* `target_field_name`
* `source_record_type`
* `source_field_name`
* `notes`
