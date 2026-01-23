---
description: >-
  Configuration reference for non-default HTTP(S) integration API paths and
  verbs
---

# Web Service Endpoints

Integration Gateway supports building web services that execute integrations via user-defined URL paths and methods.  These enables the construction of traditional RESTful web services as explained in the [building-a-restful-crud-web-service](../tutorials/building-a-restful-crud-web-service/ "mention") guide.

{% hint style="warning" %}
Only URL paths starting with `/api/` or `/rest/` are currently supported.
{% endhint %}

## Create a Web Service Endpoint

To create a web service endpoint for an integration:

1. Navigate to the _Build_ page using the navigation sidebar.
2. Right-click on the integration you want to create a webservice endpoint for. Click _Configure._
3. Exapnd the _Webservice Endpoints_ section, then click _+ Add Webservice Endpoint_
4. Fill the fields, then click _Save._

<figure><img src="../.gitbook/assets/Screenshot 2024-10-09 at 4.38.16 PM.png" alt=""><figcaption></figcaption></figure>

## Using a Webservice Endpoint

The full URL for a webservice endpoint is `your.environment.name.sandboxbanking.com/prefix/path_name` where

* &#x20;`prefix` is either `api` or `rest`&#x20;
* `path_name` is the path name configured in the webservice endpoint, **not the pathname of the integration**

## Fields

<table><thead><tr><th width="179">Name</th><th width="323">Example Value (s)</th><th>Description</th></tr></thead><tbody><tr><td>Path</td><td>Example: <code>q2/apps</code></td><td>Specifies the endpoint's URL path following the initial <code>/api/</code> or <code>/rest/</code> root fragment.<br><br>Do not include a leading <code>/</code> . </td></tr><tr><td>Method</td><td><code>GET</code>, <code>POST</code>, <code>PUT</code>, <code>PATCH</code>, <code>DELETE</code></td><td>Specifies the HTTP method used for calling the API.</td></tr><tr><td>Prefix</td><td><code>/api</code>, <code>/rest</code></td><td>Specifies the endpoint URL's root path fragment.  All web service endpoints must start with either <code>/api/</code> or <code>/rest/</code>.</td></tr><tr><td>Description</td><td>Example: Retrieves applications from Q2 Gro online deposit opening system</td><td>Description of the web service endpoint.</td></tr><tr><td>Response content type</td><td><code>application/json</code>, <code>application/html</code>, <code>text/xml</code>, <code>text/html</code>, <code>application/soap+xml</code></td><td>Specifies the integration response type. </td></tr></tbody></table>
