# Admin Components

### Global Config

The base configuration for Integration Gateway.

* Email configuration for reports
* Storing of request & response payloads to Integration Gateway

### Integration Configs

Configures email notifications and data retention. [integration-config.md](../reference/integration_components/integration-config.md "mention")

### Integration Permissions

User permissions must be set per integration. Options are `read`, `write`, `execute`, and `debug`. By default, the user who creates the integration has full integration permissions.

* `read` — Allows the user to see the contents of the integration, including its Service Requests, Field Mappings, Value Mapping Sets, etc.
* `write` — Allows the user to modify any aspect of the integration
* `execute` — Allows the user to manually trigger the integration using the _Run Integration_ feature
* `debug` — Allows the user to see run histories from past integration runs

### Adapter Configurations

Adapter configurations securely store credentials for use with service requests. Please see associated adapter documentation for the correct fields to use.

### Adapter Configuration Bindings

The relationship between the integration and adapter configuration. Integrations will use the provided adapter config to make any associated service request calls. This configuration can be overridden within integrations with special variable [**adapter\_config**](/broken/pages/CWJw9yOYSuXOFGJaoSTS#adapterconfig)**.**

### Web Service Endpoints

Integrations can be triggered from their HTTP POST method for the integration `path_name`.  This endpoint exposes integrations to be called using all available HTTP verbs at use-defined paths.  See details at [web-service-endpoints.md](../reference/web-service-endpoints.md "mention").
