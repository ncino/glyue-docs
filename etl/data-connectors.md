# Data Connectors

Integration Gateway ETL uses Data Connectors to manage access to external systems, such as SFTP servers and CRMs like HubSpot and Creatio. The requirements for each data connector differs based on the specific system, but the [basics are outlined further down on this page](data-connectors.md#system-specific-connector-requirements).

A data connector only manages access credentials and does not specify the exact records being accessed or modified. The exact records are instead specified on the workflow's configuration when the connector is selected. This allows the same connector to be used across multiple workflows — it can grant access to different files for different workflows from the same SFTP server, or write to different objects in a HubSpot instance, for example.

{% hint style="info" %}
Some data connectors may be "source only" or "destination only". &#x20;
{% endhint %}

### Development vs. Production Credentials

A connector actually contains two sets of credentials — one for development purposes, and another for production uses. While Integration Gateway ETL does not enforce what type of system you enter credentials for, it is **strongly recommended** that you adhere to best practices and only enter SDLC-appropriate credentials.

A data connector only requires that one set of credentials is populated. However, missing credentials for one system will disable certain features, detailed below:

* Features that require **Production** credentials
  * Setting a schedule for a workflow&#x20;

{% hint style="info" %}
Scheduled workflows always run using production credentials.
{% endhint %}

* Features that require **Development** credentials
  * Running a _Draft_ workflow

### System-specific Connector Requirements

**SFTP Servers**

<table><thead><tr><th width="141.2109375">Field</th><th>Meaning</th><th>Example</th></tr></thead><tbody><tr><td>Hostname</td><td>The URL of the SFTP server</td><td>sftp.sandboxbanking.com</td></tr><tr><td>Port</td><td>The exposed port of the SFTP server. Defaults to 22</td><td>22</td></tr><tr><td>Username</td><td>The username of the account to be used to access the SFTP server</td><td>Varies</td></tr><tr><td>Authentication Method</td><td>How the account should authenticate with the SFTP server. Is either a password, or an authentication certificate.</td><td>Password or Private Key (Certificate)</td></tr><tr><td>Authentication Credentials</td><td>Depending on the authentication method, either the password or the certificate.</td><td>Varies</td></tr></tbody></table>



**HubSpot**

Integration Gateway ETL's HubSpot connector leverages browser-based OAuth, meaning connection only requires logging into your HubSpot account from Integration Gateway ETL.

The **Date Format** field specifies the date format sent to HubSpot, as HubSpot requires a consistent date format across all date values. In most cases, the default value is appropriate and does not need to be changed.



**Oracle SQL**

