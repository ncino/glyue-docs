# Governance Reports

Integration Gateway provides governance reports that details the state of  [integration execution permissions](permissions/#integration-permissions) across all users. The report provides two views:

* **Access Report** shows a list of all currently active permissions for all active users in the system.
* **Permission Log** shows a ledger of all permission changes for the selected timespan. This includes both permission grants and revocations.

The meaning of each column in the report is detailed below:

### User / Service Account Access Report

<table><thead><tr><th width="170">Column</th><th>Meaning</th></tr></thead><tbody><tr><td>User</td><td>The username of the user that the permission belongs to</td></tr><tr><td>Organization</td><td>The organization (if any) the user belongs to</td></tr><tr><td>Integration</td><td>The name of the integration</td></tr><tr><td>Granted by</td><td>The username of the user who granted the integration permission</td></tr><tr><td>Granted on</td><td>The date and time (UTC) of the permission change</td></tr><tr><td>Permission Type</td><td>Whether the permission was granted on an individual basis (<code>user</code>) or via the user's membership in a group (<code>group</code>)</td></tr><tr><td>Group</td><td>If the permission was granted via a group, the name of the group that had its permissions changed</td></tr></tbody></table>

### Permission Log

<table><thead><tr><th width="122">Column</th><th>Meaning</th></tr></thead><tbody><tr><td>User</td><td>The username of the user that the permission change occured to</td></tr><tr><td>Name</td><td>The name of the integration or group</td></tr><tr><td>Action</td><td><ul><li><code>grant</code> — the user was granted the permission</li><li><code>revoke</code> — the user's permission to the integration was revoked</li><li><code>added</code> — the user was added to the group</li><li><code>removed</code> — the user was removed from the group</li></ul></td></tr><tr><td>Type</td><td><ul><li><code>IntegrationPermission</code> — the change occurred to the user's individual access to the integration</li><li><code>GroupIntegrationPermission</code> — the change occurred because the group the user is a part of had its permission granted/revoked</li><li><code>Group</code> — the change occured to a user's membership in a group</li></ul></td></tr><tr><td>Time</td><td>The date and time (UTC) of the change</td></tr></tbody></table>
