# PostgreSQL

## Supported Tasks

* Extract
* Load

## Fields

<table><thead><tr><th width="159.62109375">Field</th><th width="314.109375">Description</th><th>Example</th></tr></thead><tbody><tr><td>Host</td><td>The hostname of the PostgreSQL server.</td><td><code>postgresql.example.com</code></td></tr><tr><td>Port</td><td>The port the PostgreSQL server is listening on.<br><br>By default, it's <code>5432</code>.</td><td><code>5432</code></td></tr><tr><td>Username</td><td>The name of the PostgreSQL user.</td><td><code>postgres</code></td></tr><tr><td>Password</td><td>The password associated with the specified username.</td><td><code>MyPassword1!</code></td></tr><tr><td>Database</td><td>The name of the database to connect to.</td><td><code>my_db</code></td></tr><tr><td>Connect Timeout</td><td>The timeout, in seconds, when connecting to the database.<br><br>By default, it's 30 seconds.</td><td><code>30</code></td></tr><tr><td>SSL Mode</td><td>If the database should be connected to via SSL, this should be enabled.<br><br>For more details on the various SSL modes, check out <a href="https://www.postgresql.org/docs/current/libpq-connect.html#LIBPQ-CONNECT-SSLMODE">the PostgreSQL documentation</a>.</td><td><code>Disable</code></td></tr><tr><td>SSL Client Certificate</td><td>The SSL certificate for the client connecting to the server.<br><br>Only required when SSL Mode is enabled.</td><td><code>client-cert.pem</code></td></tr><tr><td>SSL Client Key</td><td>The private SSL key for the client.<br><br>Only required when SSL Mode is enabled.</td><td><code>client-key.pem</code></td></tr><tr><td>SSL Root Certificate</td><td>The root SSL certificate for the database server.<br><br>Only required when SSL Mode is enabled.</td><td><code>server-ca.pem</code></td></tr></tbody></table>
