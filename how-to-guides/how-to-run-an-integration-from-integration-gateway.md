---
description: Using the Integration Runner
---

# How to Run an Integration from Integration Gateway

Running an integration in Integration Gateway involves composing a payload that matches the integration's requirements, then sending that payload to the integration. This can be used as the primary way to run an integration, or to mimic an expected call from a 3rd party service for testing purposes.

## Creating a request

Integration Gateway makes it easy to compose the exact request that your integration expects.&#x20;

1. To start, right-click on the integration on the _Build_ page and click _Run Integration_ to bring up the _Run Integration_ moda&#x6C;_._
2. If the integration has webservices, use the `Method`, `Prefix`, and `Webservices` dropdowns along the top of the page to select the webservice you wish to trigger.
3. Configure the content of the request across the _Payload, Headers, Query Parameters,_ and _Path Parameters_ (see below for further details)

{% hint style="info" %}
To learn more about [webservices, see this reference](../integration-gateway-platform-reference/integration_configuration.md#web-service-endpoints).
{% endhint %}

## Request customization

### **Payload**

_Payload_ refers to information sent in the body of the request. This information can come in various forms, outlined below:

* JSON: A JSON editor will appear to input JSON-structured content
* XML: An XML editor will appear to input XML-structured content
* Multipart: Accepts a combination of uploaded files (up to 10MB) and form data
* Binary: Accepts a single file (up to 10MB)
* Empty: The request body will contain no data

<figure><img src="../.gitbook/assets/Screenshot 2024-02-20 at 1.55.54 PM.png" alt=""><figcaption><p>The payload interface, with Multipart selected.</p></figcaption></figure>



### **Headers**

Headers will be automatically populated based on the content of the payload and other details of the integration. When uploading multipart or binary payloads, the _Integration Runner_ will suggest appropriate headers based on the file type(s) that are uploaded.

Some headers are omitted from the display and set automatically when the integration is sent. Certain headers will be ignored even if specified, for security reasons. _See the full list ignored/hidden headers in the_[ _Appendix_](how-to-run-an-integration-from-integration-gateway.md#appendix)_._

<figure><img src="../.gitbook/assets/Screenshot 2024-02-20 at 1.52.15 PM.png" alt=""><figcaption><p>The Headers interface.</p></figcaption></figure>

### **Query Parameters**

To set query parameters (information passed in via the url, e.g. `sandbox.com/endpoint?flavor=chocolate` ), enter their key and value. New pairs can be added using the green plus button at the top-left.&#x20;

Notice the url-encoded form of the query parameters will update in the displayed URL along the top of the _Integration Runner_.

<figure><img src="../.gitbook/assets/Screenshot 2024-02-20 at 1.47.17 PM.png" alt=""><figcaption><p>The query parameters interface.</p></figcaption></figure>



### **Path Parameters**

Integrations with webservices may have variables in the webservice path. These are represented by `{variable}` in the path name. As you enter values for each path parameter(s), you should see the value(s) update in the displayed URL along the top of the _Integration Runner_.

{% hint style="info" %}
All path parameters must have specific values before the request can be executed.
{% endhint %}

<figure><img src="../.gitbook/assets/Screenshot 2024-02-20 at 1.49.19 PM.png" alt=""><figcaption><p>The path parameters interface.</p></figcaption></figure>

Note that path parameters cannot be added/edited/deleted from this page — modifications to the path occur on the webservice itself.



## Re-running an integration

Oftentimes it is useful to re-use the same request, either for simplicity or debugging purposes. This is done by hovering over an entry in the _Run History_ and clicking the re-run icon.&#x20;

The _Run Integration_ modal will be pre-filled with the same request details as the selected run. If the selected run involved sending Binary or Multipart files, the same files will be re-used by default. Feel free to change the pre-filled values as desired.



<figure><img src="../.gitbook/assets/Screenshot 2024-02-20 at 4.00.03 PM.png" alt=""><figcaption><p>The re-run icon within the <em>Run History</em> list.</p></figcaption></figure>



## Appendix

**Ignored headers (will not be sent even if set in Headers tab)**

* `accept-encoding`
* `connection`
* `content-length`
* `host`
* `user-agent`

**Hidden headers (automatically set by Gylue on integration execution)**

* `authorization`
* `content-type`
* `origin`
* `referer`
* `x-csrftoken`
