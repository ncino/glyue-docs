# fitem/fidx

Provides access to the value (`fitem`) and index (`fidx`) when iterating through a list using the [`include_for_each`](../../integration_components/field-mapping.md#include_for_each) column on a [field mapping](../../integration_components/field-mapping.md).&#x20;

These variables are only populated when the `include_for_each` column is set, and are only accessible in its `field` and `value` columns. They are usually used to populate lists or dictionaries for a service request's fields.

{% hint style="info" %}
Read more about the `include_for_each` column [here](../../integration_components/field-mapping.md#include_for_each).
{% endhint %}

```
# Given
include_for_each = ["foo", "bar", "baz"]

# First iteration
fitem ==> "foo"
fidx ==> 0

# Second iteration
fitem ==> "bar"
fidx ==> 1

etc...
```



**Example usage**

```
# Given
include_for_each = [
    {"name": "Sally", "email": "sally@email.com"},
    {"name": "Benjamin", "email": "benjamin@email.com"},
    {"name": "David", "email": "david@email.com"}
]

# To fill a theortical "all_emails" field, use
field: all_emails[fidx]
value: fitem['email']

 # Results in
 ["sally@email.com", "benjamin@email.com", "david@email.com"] 
```

