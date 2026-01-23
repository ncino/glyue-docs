# debug

debug`(value: any, label: str, capture_stack_trace: bool) -> None`&#x20;

Creates a Run History item with the specified variable and optional label.&#x20;

This is a functional equivalent of `print` . It exists because there is no `stdout` interface within Integration Gateway, so all output must be intentionally called to be surfaced in the Run History.

```python
a = ["SAMPLE"]
debug(a, label="Sample array")
```
