# Banking Core Connectivity Guide

Integration Gateway integrates across your technology stack, including on-prem systems. The most common type of on-prem system is a core, and often, its associated middleware.&#x20;

{% hint style="info" %}
An on-premises system is a system that is on its own, internal network, and is not accessible by systems that are not on that network.
{% endhint %}

Below are the steps outlining how Integration Gateway establishes connectivity with an on-prem core. Some details may differ depending on the specifics of a core/middleware, but the high level steps remain the same.

At a high level, there are three steps:&#x20;

1. Establish a VPN between Integration Gateway's servers and your bank’s internal network
2. Configure middleware for Integration Gateway API access
3. Create a service account for Integration Gateway on the core

Let’s explore each step in detail below.

***

### 1. Establish a VPN

A VPN (Virtual Private Network) is a private “tunnel” between systems, in this case, Integration Gateway servers and your on-prem core. Integration Gateway uses a VPN to communicate with your core to keep the information secure and private. Setting up a VPN will require coordination between your IT team and Integration Gateway's network engineers.

<figure><img src="../.gitbook/assets/Sandbox Integration Architecture simplified (2).png" alt=""><figcaption><p>VPN Infrastructure Diagram</p></figcaption></figure>

Once the VPN is set up, your IT team will expose certain systems to the VPN so they can be accessed by Integration Gateway. This can be your core and middleware, a database, or other systems running on your internal network.&#x20;

{% hint style="info" %}
Some cores may not be hosted on-prem, but instead as a managed service in a 3rd-party datacenter, such as one managed by the core’s vendor. \
\
For these setups, Integration Gateway does not require a VPN to your network, and will instead work with you and the vendor to establish access to your core via the managed datacenter.
{% endhint %}

### 2. Configure Integration Gateway in the core’s middleware

Most cores have an associated system called a “middleware” that sits between the core and external systems, governing access to the core. Integration Gateway will need to be configured as an application in the middleware, along with the permissions necessary to execute your use cases.&#x20;

Sandbox staff will work with you to determine the correct set of permissions for your specific needs.

{% hint style="info" %}
If you do not have a middleware instance set up, you will need to work with your core vendor to install/license one before Integration Gateway can integrate with your core.
{% endhint %}

### 3. Create a service account

Once Integration Gateway is configured in the middleware, it needs an account on the core in order to read data and make changes. This is called a “service account”, to indicate that it’s being used by a service (Integration Gateway) instead of a regular employee of the bank.&#x20;

Integration Gateway's service account will also need the appropriate permissions to execute its intended use cases. These will be similar to, but may not exactly mirror, the permissions set earlier in the middleware configuration step.

{% hint style="info" %}
If you are building multiple integrations with separate purposes, it may be appropriate to create separate service accounts for each integration. Consult with your Sandbox Banking engineers to determine the best approach.
{% endhint %}

***

Once a VPN, middleware configuration, and core service account are set up, you will have the necessary information to set up the access credentials within Integration Gateway. This can be done via the [configuration wizard](https://glyue.docs.sandboxbanking.com/reference/ui_walkthrough#step-2-systems-adapters) on Integration Gateway's Build page.
