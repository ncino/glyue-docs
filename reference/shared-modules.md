# Shared Modules

The Shared Module feature allows for importable code to be stored outside of an integration. This can be useful for authoring libraries of functionality, or generally for writing code that is useful across several integrations.

## Permissions

Users must have the `can_use_shared_modules` permission in order to access this page.

## Anatomy

Users with the permission will see the "Shared Modules" page appear in their left sidebar

<figure><img src="../.gitbook/assets/Screenshot 2025-12-19 at 11.06.20 AM.png" alt=""><figcaption></figcaption></figure>

The **Module Selection Pane** can be used to select an existing or new module\
Each module has a **Module Name** which is used to reference the module within the integration.\
The **Module Code** pane contains the source code for the module.\
The **Bound Integrations** selection controls which integrations can use the module.\
The **Save** button saves the changes to the Shared Module.

### Importing and Using a Shared Module

Shared modules must be bound to an integration before they can be imported.

\
Shared modules behave much like python's built-in concept of modules.\
They can be imported from within integration hooks or from within other modules using the [import\_module](special_functions/import_module.md) special function

For example, your integration hook might contain

```
import_module("money_utils")
debug(money_utils.format_currency(123), "Formatted currency")
```

To use your money\_utils throughout the entirety of an integration, you may want to `keep` it, `keep(money_utils = import_module("money_utils"))`

### Shared Module Capabilities

Shared Modules should generally be used for defining functions and classes for use by your integrations. They can use many of the same [special functions](special_functions/) and [variables](special_variables/) that are available in other integration code hooks, however `keep` is not supported within a shared module.
