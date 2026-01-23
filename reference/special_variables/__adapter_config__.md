# \_\_adapter\_config\_\_

`__adapter_config__ -> {}`&#x20;

Exposes properties of an adapter config to be overridden using field mappings or in lifecycle hooks.&#x20;

{% hint style="info" %}
Overrides must occur during the `before_prepare_request_hook` or later. See the [full service request hook lifecycle order here](../integration-lifecycle.md#service-request-overview).
{% endhint %}

A service request's payload may optionally have an  `__adapter_config__` field added to it. When present, the keys and values of the  `__adapter_config__` dictionary override the corresponding entries in the adapter config that would normally be used.

{% hint style="warning" %}
Values stored within Field Mappings do not have the same encryption methods as fields set directly on adapter configs. &#x20;

**Do not put sensitive information, including API secrets / passwords, in field mappings or lifecycle hooks.**
{% endhint %}

The `__adapter_config__` field can either be set directly on the payload in a hook:

```
my_service_request.payload.__adapter_config__ = {
    "username" : "override_username",
    "host" : "other.website.com"
}
```

Or equivalently, individual fieldmappings can be created like below

```
FIELD: __adapter_config__.username
VALUE: "temporary_username"

FIELD: __adapter_config__.host
VALUE: "other.website.com"
```
