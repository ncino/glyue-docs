# Service Request

### integration

<mark style="color:blue;">`relationship`</mark> - <mark style="color:red;">`required`</mark>

The integration that the service request is related to.

### parent\_servicerequest

<mark style="color:blue;">`relationship`</mark> - <mark style="color:red;">`required`</mark>

If specified, the service request will be attached to another service request.

### sequence

<mark style="color:yellow;">`integer`</mark> - <mark style="color:red;">`required`</mark>

Order in which the service requests associated with the integration will execute, from lowest (first) to highest (last). Service requests with the same sequence will be run in arbitrary order.

### system

<mark style="color:yellow;">`string`</mark> - <mark style="color:red;">`required`</mark>

The external system being called by the service request. Banking-specific systems are identified by name, otherwise the `HTTP` adapter is suitable for connecting to arbitrary systems.&#x20;

### service\_name

<mark style="color:yellow;">`string`</mark> - <mark style="color:red;">`required`</mark>

The API service being called within the target system. Usage specifics vary from system to system.

### formula\_variable

<mark style="color:yellow;">`string`</mark> - <mark style="color:red;">`required`</mark>

The name of the variable that will contain the `request`and `response` values of this service request. If the service request uses `call_for_each` , `request`and `response` will be lists of objects.&#x20;

This variable can be accessed in subsequent service requests, field mappings, or lifecycle hooks of the integration as a means to pass data.

### call\_if

<mark style="color:yellow;">`code block`</mark> - <mark style="color:orange;">`optional`</mark>

This expression must return a boolean. If it resolves to `True` , the service request will be run. If left empty, the service request will always run.

### call\_for\_each

<mark style="color:yellow;">`iterable`</mark> - <mark style="color:orange;">`optional`</mark>

Specifies an iterable. The service request will run once for every entry in the iterable. Within other code blocks or field mappings of the service request, the specific entry in the iterable can be accessed using [`sritem`](../special_variables/#iterable-within-integrations-xx-item-and-xx-idx)and its index via [`srindex` ](../special_variables/#iterable-within-integrations-xx-item-and-xx-idx).

Resolves before the `call_if` block.

### abort\_after\_execute\_request\_failure

<mark style="color:yellow;">`boolean`</mark> - <mark style="color:orange;">`optional`</mark>

Determines whether the integration should quit if the call to the external system fails. By default, the integration will not quit (`false`). When set to `true` , the integration's `after_overall_failure_hook` will still run, as well as the service request's `after_execute_request_failure_hook`.&#x20;

### skip\_execute\_if\_no\_sub\_requests

<mark style="color:yellow;">`string`</mark> - <mark style="color:orange;">`optional`</mark>



### message\_substitution\_name

<mark style="color:yellow;">`string`</mark> - <mark style="color:orange;">`optional`</mark>

Replaces the name of the service with specified string in error messages. Useful to improve readility of errors for non-technical customers.

### notes

<mark style="color:yellow;">`string`</mark> - <mark style="color:orange;">`optional`</mark>

Documentation field, internal to Integration Gateway.



### Lifecycle Hooks

These are code blocks that are run during lifecycle of the service request. They are equivalent in function, differentiated only by the time at which they are executed. See the [Service Request Lifecycle](../integration-lifecycle.md#service-request-lifecycle) for a visual explanation of hook timing.

None are required for a service request to function.&#x20;

* before\_prepare\_request\_hook
* after\_prepare\_request\_success\_hook
* after\_prepare\_request\_failure\_hook
* before\_execute\_request\_hook
* after\_execute\_request\_success\_hook
* after\_execute\_request\_failure\_hook
* after\_overall\_success\_hook
* after\_overall\_failure\_hook
