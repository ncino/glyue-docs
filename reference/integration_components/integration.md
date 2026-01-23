# Integration

### active

<mark style="color:yellow;">`boolean`</mark> - <mark style="color:red;">`required`</mark>

Defines whether the integration can be run at all. If set to `false` , the integration **cannot** be run via an HTTP request (regardless of the value of `http_api`), and the integration **cannot** be run via [the `callint` function](../special_functions/#callint).&#x20;

### path\_name

<mark style="color:yellow;">`string`</mark> - <mark style="color:red;">`required`</mark>

The exposed endpoint for the integration. Integrations are called from `{host}/integrations/execute/{path_name}`

### description

Documentation field providing an overview of the integrations functions.

### http\_api

<mark style="color:yellow;">`boolean`</mark> - <mark style="color:red;">`required`</mark>

Determines whether or not an integration can be run via an HTTP request over the web.&#x20;

When enabled, the integration can be called from `{host}/integrations/execute/{path_name}`  or any configured [webservice endpoints](../web-service-endpoints.md).&#x20;

When disabled, the integration can only be called from another Integration Gateway integration, via the[ `callint` function](../special_functions/#callint).

### run\_async

<mark style="color:yellow;">`boolean`</mark> - <mark style="color:red;">`required`</mark>

Controls whether to run the integration synchronously. If `true` , Integration Gateway will immediately return a `200` response without waiting for the integration to finish executing.

Useful for long-runnings tasks that may otherwise cause timeouts on clients, or in combination with `callint` to spawn additional processes.



## Lifecycle Hooks

For a visual explanation of the timing of these hooks, see the [Integration Lifecycle diagram](../integration-lifecycle.md#integration-lifecycle).

### before\_hook

<mark style="color:green;">`expression`</mark> - <mark style="color:orange;">`optional`</mark>

A block of code that executes prior to the initialization of the integration.

### on\_failure\_hook

<mark style="color:green;">`expression`</mark> - <mark style="color:orange;">`optional`</mark>

A block of code that runs only upon an integration failure, prior to the finally hook.

### on\_success\_hook

<mark style="color:green;">`expression`</mark> - <mark style="color:orange;">`optional`</mark>

A block of code that runs only upon a successfully completed integration, prior to the finally hook.

### finally\_hook

<mark style="color:green;">`expression`</mark> - <mark style="color:orange;">`optional`</mark>

A block of code that executes prior to the completion of an integration. This block will always run regardless of integration status.



## Swagger

Integration Gateway has an integrated swagger page to allow for easy endpoint execution from within Integration Gateway. The swagger request and response fields allow for the specification of swagger request bodies and sample responses. These must be formatted in line with the [OpenAPI Specification](https://spec.openapis.org/oas/v3.1.0). Swagger

#### swagger\_request

[Swagger Request Body](https://swagger.io/docs/specification/describing-request-body/) - This field is scoped to the `requestBody` of the OAS 3 specification

```python
{ "content": {
    "application/json": {
        "schema": {
            "type": "string", 
            "example": "sample string input" 
            } 
        } 
    } 
}
```

#### swagger\_response

The input is scoped to the `responses` on the OAS 3 specification.

```python
{ "200": {
     "description": "Sample Response", 
     "content": { 
           "application/json": { 
                "schema": {
                     "type": "string", 
                     "example": 
                     "Example Object" 
                     }
                }
           }
      } 
}
```
