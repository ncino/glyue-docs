# Logging

Integration Gateway integrations generate logs during their execution that can be reviewed for troubleshooting purposes when needed. These logs can be found on the left-hand sidebar under the _Logs_ tab for [superuser accounts](permissions/#user-type).

### Types of Logs

Logs are stored at two levels:

* INFO
* DEBUG

INFO logs are always generated when an integration runs and contains basic start, end, and success/failure information.

DEBUG logs provide a further level of detail, including raw request objects. Because DEBUG log entires may contain sensitive information, they must be proactively enabled before the logs are recorded.

### Enabling DEBUG Logging

DEBUG logging is enabled using the toolbar at the bottom of the _Log_ page. It can be enabled for 24 hours or 7 days at a time, and will automatically turn off at the end of that period. If DEBUG logging needs to be enabled for longer, an active DEBUG log session can be extended by clicking the _Change Debug Logging Status_ button during an active session and choosing the desired duration.

When DEBUG logging is enabled, the DEBUG log entries are visible to all users who have access to the _Log_ page.

<figure><img src="../.gitbook/assets/Screenshot 2025-02-26 at 11.17.58 AM.png" alt=""><figcaption><p>The <em>Log</em> page toolbar</p></figcaption></figure>

### Viewing Logs

The _Log_ page includes a _Log Level Filter_ that controls which log entires are displayed:

* INFO level shows only INFO level log entries
* DEBUG level shows both INFO and DEBUG level log entries
