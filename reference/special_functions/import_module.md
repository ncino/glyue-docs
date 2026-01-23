# import\_module

See [shared-modules.md](../shared-modules.md "mention")

`import_module(module_name : str) -> ModuleType` imports a shared module and makes it available for use in the current hook.

**Example usage:**

Import a module and use it in the current hook:

```
import_module("my_module")
result = my_module.test_function()
```

To use a module throughout an integration you can either reimport it in every hook it is used, or [keep](keep.md) it:

```
keep(my_module = import_module("my_module"))
```
