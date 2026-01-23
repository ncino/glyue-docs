# SFTP

## Supported Tasks

* Extract
* Load

## Fields

<table data-full-width="false"><thead><tr><th width="142.015625">Field</th><th width="419.9296875">Description</th><th>Example</th></tr></thead><tbody><tr><td>Hostname</td><td>The host of the SFTP server.<br><br>It's assumed the host uses HTTPS.</td><td><code>sftp.example.com</code></td></tr><tr><td>Port</td><td>The port the SFTP server is listening on.<br><br>By default, the port is 22.</td><td><code>22</code></td></tr><tr><td>Username</td><td>The name of the SFTP user.</td><td><code>testuser</code></td></tr><tr><td>Auth Type</td><td>The method by which to perform authentication.<br><br>Either "Password" or "Private Key".</td><td><code>password</code></td></tr><tr><td>Password</td><td>The password associated with the specified username.<br><br>Only required when "Auth Type" is set to "Password".</td><td><code>MyPassword1!</code></td></tr><tr><td>Private Key</td><td>A file containing the OpenSSH private key for the server. Usually, this will be a <code>.pem</code> file.<br><br>Only required when "Auth Type" is set to "Private Key".</td><td><code>my_private_key.pem</code></td></tr><tr><td>Client Key Type</td><td>The algorithm used to generate the private key.<br><br>Currently, the supported algorithms are: <code>rsa</code>, <code>dss</code>, <code>ecdsa</code>, <code>ed25519</code>.<br><br>If you don't know what algorithm was used to generate the key, there's also <code>auto</code> which attempts to identify the key type for you.</td><td><code>auto</code></td></tr></tbody></table>

