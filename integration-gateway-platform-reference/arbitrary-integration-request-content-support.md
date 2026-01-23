---
description: >-
  Integration Gateway supports building integration web service APIs that
  receive any arbitrary binary content (e.g. documents, bulk CSV data files,
  audio/video streams, archives)
---

# Arbitrary Integration Request Content Support

Integration execution endpoints can be passed body content that isn't a single JSON or XML payload. When this occurs, the request's body is converted into one or multiple `GlyueFileHandle` objects and then passed into Integration Gateway's integration engine for processing.

{% hint style="info" %}
The term "file upload integration" is used to reference integrations that accept input data that isn't represented as JSON or XML.
{% endhint %}

Execution requests to file upload integrations are subject to the following rules:

<table><thead><tr><th width="157">HTTP Method</th><th width="258">HTTP Request with Single File</th><th>HTTP Request with Multiple Files</th></tr></thead><tbody><tr><td>POST</td><td><mark style="color:green;">supported</mark></td><td><mark style="color:green;">supported</mark></td></tr><tr><td>PUT</td><td><mark style="color:green;">supported</mark></td><td><mark style="color:red;">rejected (415 unsupported media type)</mark></td></tr><tr><td>PATCH</td><td><mark style="color:green;">supported</mark></td><td><mark style="color:red;">rejected (415 unsupported media type)</mark></td></tr><tr><td>GET</td><td><mark style="color:yellow;">accepted but body is ignored</mark></td><td><mark style="color:yellow;">accepted but body is ignored</mark></td></tr><tr><td>DELETE</td><td><mark style="color:yellow;">accepted but body is ignored</mark></td><td><mark style="color:yellow;">accepted but body is ignored</mark></td></tr></tbody></table>

{% hint style="warning" %}
By default, integration endpoints only accept POST requests.  Web Service Endpoints must be created to enable integration execution via other HTTP methods as documented in the [building-a-restful-crud-web-service](../tutorials/building-a-restful-crud-web-service/ "mention") how-to guide and the [web-service-endpoints.md](../reference/web-service-endpoints.md "mention") reference page.
{% endhint %}

## Content-Type Handling

Integration Gateway parses integration web service HTTP(S) bodies in accordance with the request's `Content-Type` header value.  The following behavior is enforced:

<table><thead><tr><th width="294">HTTP Request Content-Type(s)</th><th>Body Parsing Behavior</th></tr></thead><tbody><tr><td><code>application/json</code></td><td>Parsed as JSON into Pythonic data structure</td></tr><tr><td><code>application/xml</code> or <code>text/xml</code></td><td>Parsed as XML into Pythonic data structure</td></tr><tr><td><code>multipart/form-data</code></td><td>Parsed into one or more <code>GlyueFileHandle</code> objects in accordance to <code>multipart/form-data</code> standards</td></tr><tr><td><code>*/*</code> or any other value</td><td>Parsed into one <code>GlyueFileHandle</code> object</td></tr></tbody></table>



{% hint style="info" %}
It is possible to submit a single file in a `multipart/form-data` request.
{% endhint %}

## Multipart / Form-Data Content Requirements

Integration Gateway assumes that the name of the field in the form data is the name of the file in that field.

Non-file form data will appear in `input.payload` while the files will appear in `input.files` as shown below.

<table><thead><tr><th width="259"></th><th></th></tr></thead><tbody><tr><td><code>loan</code></td><td><code>FISCHER-20230531</code></td></tr><tr><td><code>note</code></td><td><code>Documents received from applicant today</code></td></tr><tr><td><code>driver_license.tiff</code></td><td>(file data)</td></tr><tr><td><code>bank_statement_01.pdf</code></td><td>(file data)</td></tr></tbody></table>

```
{
    ...
    "headers": {...},
    "payload": {
        "loan": "FISCHER-20230531",
        "note": "Documents received from applicant today"
    },
    "files": {
        "driver_license.tiff": <GlyueFileHandle ... >,
        "bank_statement_01.pdf": <GlyueFileHandle ... >,
    },
    ...
}
```

## Single Binary / File Upload Requirements

Content type for these requests will vary depending on the file being sent.  Some common content types that apply are `application/zip`, `application/pdf`, `image/png`, `audio/mpeg`, `video/mp4`, `application/octet-stream`.

### Content-Disposition Header

In order for Integration Gateway to be aware of the file's name, the request must include it within the `Content-Disposition` in a standards-compliant manner like the following:

<table data-header-hidden><thead><tr><th width="240"></th><th></th></tr></thead><tbody><tr><td><code>Content-Disposition</code></td><td><code>attachment; filename=name_goes_here.docx</code></td></tr></tbody></table>

The above header would result in an `input` object with the following `files` content:

```
"payload": {},
"files": {
    "name_goes_here.docx": <GlyueFileHandle id=1 name=name_goes_here.docx>
}
```

Without the header, the default filename `file` (with no extension) would instead be used:

```
"files": {
    "file": <GlyueFileHandle id=1 name=file>
}
```
