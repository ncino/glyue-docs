# Building an Event-Driven Integration

{% hint style="info" %}
This tutorial builds upon the work done in [**Build a Single-Step Integration**](../building-a-single-step-integration/) and [**Build a Multi-System Integration**](../building-a-multi-system-integration/)**.**&#x20;

If you haven't already, we recommend you complete those tutorials first, then return here.
{% endhint %}

In the previous two tutorials ([1](../building-a-single-step-integration/), [2](../building-a-multi-system-integration/)) we ran our integration manually from within Integration Gateway.&#x20;

In real-world use cases, we often see a different trigger model: Activity in one system causes a chain of events that results in some action in a different system. For example, a new customer created in a CRM will result in that customer being created in the banking core.

We call these **Event-Driven Integrations**, as the integration is run in response to events that call into Integration Gateway, rather than being manually run from within Integration Gateway.

In this tutorial, we will take the logic we built in the previous two tutorials and transform it into a **event-driven** architecture, triggering the integration from a third-party system.

For this tutorial, we will use a mock CRM built using Integration Gateway's [Frontends feature](../../how-to-guides/how-to-build-and-deploy-a-custom-frontend.md).&#x20;
