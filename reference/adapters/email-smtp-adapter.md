# Email SMTP Adapter

## AdapterConfigEmailSTMP <a href="#emailsmtpadapter-adapterconfigemailstmp" id="emailsmtpadapter-adapterconfigemailstmp"></a>

<table data-header-hidden><thead><tr><th width="190.33333333333331"></th><th></th><th></th></tr></thead><tbody><tr><td><strong>field</strong></td><td><strong>value</strong></td><td><strong>example</strong></td></tr><tr><td>Host</td><td><code>required</code> The host address for the email server</td><td><code>smtp.gmail.com</code></td></tr><tr><td>Port</td><td><code>required</code> The port for the email server</td><td><code>587</code></td></tr><tr><td>TLS encrypt</td><td><code>optional</code> Checkbox to either use TLS encryption for email sending or not</td><td><ul><li> </li></ul></td></tr><tr><td>Authenticate</td><td><code>optional</code> Checkbox to either use email authentication or not.</td><td><ul><li> </li></ul></td></tr><tr><td>Auth username</td><td><code>optional but required if ^ is checked</code> Username for user authentication that is attached to the email</td><td><code>mailbot@alphapack.co</code></td></tr><tr><td>Auth password</td><td><code>optional but required if ^ is checked</code> Password for user authentication that is attached to the email</td><td>aaa</td></tr></tbody></table>

## Service Request <a href="#emailsmtpadapter-servicerequest" id="emailsmtpadapter-servicerequest"></a>

| **column**   | **value**                                                                               | **example**            |
| ------------ | --------------------------------------------------------------------------------------- | ---------------------- |
| System       | `required` The system name of this adapter                                              | `EMAIL_SMTP`           |
| Service Name | `required` The name of the service, could be anything for this adapter, including `N/A` | `SendMessage` or `N/A` |

## Field Mappings <a href="#emailsmtpadapter-fieldmappings" id="emailsmtpadapter-fieldmappings"></a>

<table data-header-hidden><thead><tr><th></th><th></th><th></th><th></th></tr></thead><tbody><tr><td><strong>Field</strong></td><td><strong>Value</strong></td><td><strong>Value Type</strong></td><td><strong>example</strong></td></tr><tr><td>subject</td><td><code>required</code> The subject line of the email</td><td><code>str</code></td><td><code>f"Integration {integration_name} has failed on {date_time}"</code></td></tr><tr><td>from_address</td><td><code>required</code> The address the email is coming from</td><td><code>str</code></td><td><code>mailbot@alphapack.co</code></td></tr><tr><td>to_addresses</td><td><code>required</code> The address(es) that the email should be sent to</td><td><code>list</code></td><td><code>["test@email.com", "test2@email.com", "test3@email.com"]</code></td></tr><tr><td>body</td><td><code>required</code></td><td><code>str</code></td><td></td></tr><tr><td>attachments</td><td><p><code>optional</code> List of attachments on the email as a list of dicts</p><p>name: intended file name</p><p>type: file MIME type</p><p>payload: file content (bytes) <strong>NOT A BYTE STREAM</strong></p></td><td><code>list</code></td><td><pre><code>[
{
    "name": "Test File",
    "type": "text/html",
    "payload": &#x3C;_io.BytesIO object at 0x7g8j214f0h54>
}
]
</code></pre></td></tr></tbody></table>

## Service Request and Field Mapping Examples <a href="#emailsmtpadapter-servicerequestandfieldmappingexamples" id="emailsmtpadapter-servicerequestandfieldmappingexamples"></a>

| **System**  | **Service Name** | **Formula Variable** |
| ----------- | ---------------- | -------------------- |
| EMAIL\_SMTP | SendMessage      | send\_email\_smtp    |

#### Field Mappings Without Attachments: <a href="#emailsmtpadapter-fieldmappingswithoutattachments" id="emailsmtpadapter-fieldmappingswithoutattachments"></a>

| **Sequence** | **Field**     | **Value**                                                         | **Value Type** |
| ------------ | ------------- | ----------------------------------------------------------------- | -------------- |
| 1            | subject       | “This is a test subject”                                          | str            |
| 2            | from\_address | “mailbot@alphapack.co”                                            | str            |
| 3            | to\_addresses | \['travis.w@sandboxbanking.com”, “developers@sandboxbanking.com”] | list           |
| 4            | body          | “This is a test body for the test email!”                         | str            |

#### Field Mappings With Attachments: <a href="#emailsmtpadapter-fieldmappingswithattachments" id="emailsmtpadapter-fieldmappingswithattachments"></a>

<table data-header-hidden><thead><tr><th width="131"></th><th width="138"></th><th width="374"></th><th></th></tr></thead><tbody><tr><td><strong>Sequence</strong></td><td><strong>Field</strong></td><td><strong>Value</strong></td><td><strong>Value Type</strong></td></tr><tr><td>1</td><td>subject</td><td>“This is a test subject”</td><td>str</td></tr><tr><td>2</td><td>from_address</td><td>“mailbot@alphapack.co”</td><td>str</td></tr><tr><td>3</td><td>to_addresses</td><td>['travis.w@sandboxbanking.com”, “developers@sandboxbanking.com”]</td><td>list</td></tr><tr><td>4</td><td>body</td><td>“This is a test body for the test email!”</td><td>str</td></tr><tr><td>5</td><td>attachments</td><td><pre><code>[{
    'name': 'Test Attachment',
    'type': 'text/plain',
    'payload': byte_stream.getvalue()
}]
</code></pre></td><td>list</td></tr></tbody></table>

