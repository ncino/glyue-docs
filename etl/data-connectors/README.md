# Data Connectors

IG ETL uses Data Connectors to manage access to external systems, such as SFTP servers and CRMs like HubSpot. Each connector has different requirements, so be sure to take a look at [the connector-specific documentation](./#existing-data-connectors).

A data connector only manages access credentials and does not specify the exact records being accessed or modified. The exact records are instead specified on the workflow's configuration when the connector is selected. This allows the same connector to be used across multiple workflows—it can grant access to different files for different workflows from the same SFTP server, or write to different objects in a HubSpot instance, for example.

{% hint style="info" %}
Some data connectors may be "source only" or "destination only". &#x20;
{% endhint %}

## Development vs. Production Credentials

A connector actually contains two sets of credentials—one for development purposes, and another for production uses. While Integration Gateway ETL does not enforce what type of system you enter credentials for, it is **strongly recommended** that you adhere to best practices and only enter SDLC-appropriate credentials.

## Existing Data Connectors

{% content-ref url="sftp.md" %}
[sftp.md](sftp.md)
{% endcontent-ref %}

{% content-ref url="hubspot.md" %}
[hubspot.md](hubspot.md)
{% endcontent-ref %}

{% content-ref url="salesforce.md" %}
[salesforce.md](salesforce.md)
{% endcontent-ref %}

{% content-ref url="creatio.md" %}
[creatio.md](creatio.md)
{% endcontent-ref %}

{% content-ref url="postgresql.md" %}
[postgresql.md](postgresql.md)
{% endcontent-ref %}

{% content-ref url="oracle-sql.md" %}
[oracle-sql.md](oracle-sql.md)
{% endcontent-ref %}

{% content-ref url="amazon-s3-bucket.md" %}
[amazon-s3-bucket.md](amazon-s3-bucket.md)
{% endcontent-ref %}
