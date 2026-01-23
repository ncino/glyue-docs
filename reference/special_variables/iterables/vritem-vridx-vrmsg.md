# vritem/vridx/vrmsg

Provides access to the value (`vritem`) and index (`vridx`) when running a [validation rule](../../integration_components/validationrule.md) multiple times using the [`apply_to_each`](../../integration_components/validationrule.md#apply_to_each) column.

These variables are only populated when the `apply_to_each` column is set, and are only accessible in the validation rule's hooks.

{% hint style="info" %}
Read more about the `apply_to_each` column [here](../../integration_components/validationrule.md#apply_to_each).
{% endhint %}

{% hint style="warning" %}
Code in validation rules still has access to other special variables. If there is a naming conflict (e.g. `vritem` contains a field called payload) **then the non-vritem value wins out**.
{% endhint %}

### vrmsg

`vrmsg` is a validation rule-specific variable that contains the error message from a failed a validation rule. This is commonly used in the [`fail_hook`](../../integration_components/validationrule.md#validation-rule-hooks) of a validation rule to send the message up the integration chain.
