# How to Create a Value Mapping Set

### When to use a Value Mapping Set

Value Mapping Sets are an efficient way to set up [deterministic transformations](#user-content-fn-1)[^1] on a field in a payload. For example, two systems may have a field `state`, but the source system sends the value as the state abbreviation, while the destination expects it as a full spelling. Other common use cases include:

* Translating between internal codes and external-facing names&#x20;
  * e.g. a product code `400` may refer to the "High Yield Savings" product
* Reconciling different names, spelling, or formatting for the same concept across systems
  * e.g. `High Yield Savings` vs. `High-Yield Savings Account`
* Performing a many-to-few transformation
  * e.g. All variations on a checking account (`High School Checking`, `College Checking`, `Standard Checking`) are to be represented simply as `Checking Account`

If you find yourself writing `if-else` blocks that have more than 5 cases, a Value Mapping Set should be used instead.

### Creating a Value Mapping Set

A Value Mapping Set has two components:

* The Value Mapping Set, which a field mapping will refer to
* The actual Value Mappings that belong to the set, which detail an individual transformation case

1. On the build page, find your integration, right-click, then select _Go To > Value Mapping Set._
2. Click + _Add Row_ to create the new Value Mapping Set
3. Give your Value Mapping Set a clear name and description

The default settings on the value mapping set are sufficient for most use cases, but there are some instances where you may want to adjust them.

* If the incoming values are case sensitive (e.g. if `usa` vs `USA` are two distinct values that may both appear in the source system), set the `Case Sensitive` setting to `true`
  * Keeping Case Sensitivity off results in a more robust and forgiving integration, especially if the incoming values are manually entered in the source system
* If the incoming value has many options, but the majority result in the same output value, it can be easier to set a default value and only create Value Mappings for exceptions where the non-default value should be used. To do this, set `Use Default To Value` to `true`, then enter the desired default value in the `Default To Value` column.

```
Instead of this: 

From            To
value1          output1
value2          output1
value3          output1
value4          output2
value5          output1

------------------------

Do this:

Default To Value --> output1

From            To
value4          output2      
```

* If you don't know all possible values that may occur in the source system, it can be useful to set a default value to handle these cases, assuming such a value exists
  * If a default value is not set, any unrecognized values will be left unmodified.



4. Find your desired field mapping, and select the Value Mapping Set you created in the `Value Mapping Set` column.

{% hint style="info" %}
Once a Value Mapping Set is created, it can be used on multiple field mappings if necessary.
{% endhint %}

### Creating Value Mappings

Having created the Value Mapping Set, we can now create the component Value Mappings that comprise it. Right click on the Value Mapping Set row, the select _Go To > Value Mapping_. Click _+ Add Row_ to create a new Value Mapping. Because we opened the page via the context-menu of a Value Mapping Set, the _Value Mapping Set_ column is prefilled.

Enter the incoming value in the `From` column, then the corresponding output value in the `To` column.&#x20;

{% hint style="info" %}
If you have many Value Mappings already formatted in an external spreadsheet, you can simply create empty rows for them, then copy/paste them into the corresponding cells in Integration Gateway.
{% endhint %}

### Using Value Mapping Sets Outside of Field Mappings

Value Mapping Sets are powerful because they are the source-of-truth for the translation of a particular field. There are times when that information may be useful contexts outside of a direct field mapping, such as if logic in your integration expects the transformed value of a field.

Value Mappings Sets can still be used in these situations via the `map_value()` function, which takes in the name of the Value Mapping Set and the input value, and returns the corresponding output value.&#x20;

More about `map_value()` can be found in [its documentation](../reference/special_functions/#map_value).

[^1]: A "deterministic transformation" is one that always produces the same result if given the same input
