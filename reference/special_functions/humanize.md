# humanize

`humanize(str) -> str`

Attempts to regex match a field name to the [`Message Substitution Name`](../integration_components/servicerequest.md#message_substitution_name) for the a given integration component.

```python
## Field mapping with:
## 'Field' of 'sample.value'
## 'Message Substitution Name' of Sample
k = humanize("sample.value")
debug(k)
# will output 'Sample'
```
