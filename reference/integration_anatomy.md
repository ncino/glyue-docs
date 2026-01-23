---
description: Breakdown of the internal logic for an integration
---

# Integration Anatomy

## Integration Components

Integrations provide the wrapper for a set of composable logic with which to connect external systems.

## Integrations

### `Input`: Request body provided to the integration

Inputs begin with the request, but can be mutated throughout the life of the integration. For example, the `before_hook` can set additional variables on the input.payload. Inputs are also deeply integrated with Validation Rules, Masks, and can even be used with Value Mappings.

### `Output`: Integration response

Integration outputs can be mutated throughout the life of the integration.

> Integration outputs are often overwritten within calls to the service request. The `on_success_hook`, `on_failure_hook`, and the `finally_hook` are the most appropriate places to update the output.

Integration outputs are useful for error handling as well as returning specific payloads or status to calling systems.

Asynchronous integrations are a unique case and return an output immediately marking the start of the integration process. Required outputs to corresponding systems must be handled explicitly within the appropriate hooks during the integration.

#### State

Integrations have a success / failure state. The control of this state is flexible and can be triggered at will throughout the life of the integration using the special function [#end](special_functions/#end "mention"). Both service requests and validation rules contain the ability to set the integration into a failure state.

## Service Requests

The building blocks of an integration. Service requests are fired off in order to perform operations with connected systems. Each service is associated with an adapter creating a call to the service using the provided adapter configs. Services include external API calls and internal file transformations.

* `Adapter Request` the prepared request. It is available within the integration from the Service Request's `{formula_variable}.request`.
* `Adapter Response` the adapter system response. It is available within the integration from the Service Request's `{formula_variable}.response`.
* `Adapter Success` the state of the adapter service call.
* `Adapter Messages` Optional message component of the adapter. This field is generally used for return adapter specific error messages.

Service Requests have the ability to be fired off as a list of service requests to the same system using the `call_for_each` field. Given an iterable, the `call_for_each` will generate

* sridx: index of the service request iterable
* sritem: value of the service request iterable

| call\_for\_each                        | formula\_variable |
| -------------------------------------- | ----------------- |
| \[ {"name": "sr1"}, { "name": "sr2"} ] | people\_service   |

```python
# creates
people_service = [{
 "request": { "service_name": "", "payload": { "name": "sr1"} }
 "response": { "success": True, "payload": "", "messages": [] }
},
{
 "request": { "service_name": "", "payload": { "name": "sr2"} }
 "response": { "success": True, "payload": "", "messages": [] }
}]
```

Which means during an integration run, the current service request is equivalent to:

```
people_service[sridx] == sritem
```

## Field Mappings

{% hint style="success" %}
A new feature, [**Buildhelper**](../integration-gateway-platform-reference/buildhelper.md), is now available to assist in creating new Service Requests with pre-created Field Mappings for various external Systems / APIs.
{% endhint %}

Field mappings define the structure of the service request. Adapters generally have a few required field mappings such as `service_path` and `method`.

Field mappings contain the ability to be applied multiple times via an iterable provided to `include_for_each`. This exposes the following variables to be used within the field mapping code blocks.

* fidx: index of the field mapping iterable
* fitem: value of the field mapping iterable

Example:

| include\_for\_each | Value | Field                     |
| ------------------ | ----- | ------------------------- |
| \["test", "test2"] | fitem | fm\_body\[fidx].test\_key |

Generates the following request payload

```py
payload: {
    fm_body: [
        {test_key: "test"},
        {test_key: "test2"}
    ]
}
```

## Value Mapping Sets

Value Mapping Sets define a collection of Value Mappings. These are tied to an integration and can be accessed anywhere within the integration via the [#map\_value](special_functions/#map_value "mention") special function.

## Value Mappings

Value mappings mutate outgoing request values from one type to another. Common use cases are for assigning statuses from a numeric value to a string representation.

```
0: "Closed"
1: "Open"
2: "Active"
3: "Pending"
```

## Validation Rules

Validation Rules runs validation for input and response data. If a response or requested input payload fails to pass the validation rule, the integration can be put into a failure state or have other logic run based on the `abort_on_failure` field. Similarly to Field Mappings and Service Requests, Validation rules can also be run multiple times with the `apply_to_each` field. These fields use the context of vridx and vritem for the given iterable.

| apply\_to\_each |   |   |
| --------------- | - | - |
| vridx           |   |   |
|                 |   |   |
|                 |   |   |

```
vridx
vritem
```

## Masks

To hide sensitive information throughout the life of the integration. These are applied to either integration inputs or service request responses. Masked information will be replaced with the following "XXXXXXXXXXXX".

## Run History

Each integration run creates a unique Run History set for an integration which encapsulates a set of Run History Items detailing the exact steps taken within an integration. Each service request, historize statement, or other additional logging calls will generate a Run History Item.
