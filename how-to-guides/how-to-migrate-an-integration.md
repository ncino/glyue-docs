# How to Migrate an Integration

When developing integrations, Sandbox Banking Solution Engineers utilize separate `DEV` and `PROD` environments to write code and test changes before deploying that code to a production environment.&#x20;

When changes to an integration are ready to be deployed to the `PROD` environment, Integration Gateway provides two methods for performing a migration:

* [Migrating a whole integration](how-to-migrate-an-integration.md#migrating-a-whole-integration)
* [Migrating part of an integration](how-to-migrate-an-integration.md#migrating-part-of-an-integration)

### Migrating a Whole Integration

The _Migrate_ page (pictured below) allows a user to select integrations from a source environment (often, the `DEV` environment) to be migrated into the destination system (the current logged-in environment, often `PROD`).&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2024-10-09 at 2.38.50 PM (1).png" alt=""><figcaption></figcaption></figure>

To migrate an integration in its entirety:

1. Navigate to the _Migrate_ page using the navigation sidebar.
2. Select the _Source_ environment in the left-hand pane. Often, this is the corresponding `DEV` environment.
3. Select the integration(s) to be migrated in the right-hand pane.
4. Verify that the _Standard Migration_ option is selected, then click _Perform Migration._
5. Verify that the correct integration(s) were migrated by reviewing the migration summary (pictured below).

<figure><img src="../.gitbook/assets/Screenshot 2024-10-09 at 2.40.51 PM (1).png" alt=""><figcaption><p>A successful migration summary</p></figcaption></figure>

In the example above, we see that 1 integration, which contained 3 Service Requests, was migrated.&#x20;

{% hint style="info" %}
Migrating a whole integration will replace the copy of the integration that's currently in the environment.&#x20;
{% endhint %}



### Migrating Part of an Integration

Integration Gateway also supports migrating selected portions of an integration, while leaving the rest of the integration as-is. The selected changes are called a &#x63;_&#x68;angeset,_ and this type of migration is called a _Changeset Migration._

Changeset migrations are useful in situations such as:

* There is parallel development on different parts of an integration
* The `DEV` version of an integration contains development-specific logic that should not be migrated to a production environment

To perform a changeset migration:

1. Navigate to the _Migrate_ page using the navigation sidebar.
2. Select the _Source_ environment in the left-hand pane. Often, this is the corresponding `DEV` environment.
3. Click _View Incoming Changes._ This will open the _View Changes_ page, which resembles the _Build_ page, but includes two additional columns.\
   \
   &#xNAN;_&#x4D;igrate_ indicates whether a change should be included in the migration.\
   &#xNAN;_&#x43;hange_ describes the type of change (add / edit / delete).\
   ![](<../.gitbook/assets/image (193).png>)
4. Using the _View Changes_ page, select the changes you wish to include in the migration changeset by setting the _Migrate_ column to `true` for the selected rows.
5. When you're finished selecting specific changes, click _Save Change Set_ on the toolbar along the bottom right. This will show a summary of the selected changes, along with a text field to optionally name your change set. Enter a descriptive name for the changes.

<figure><img src="../.gitbook/assets/image (192).png" alt="" width="510"><figcaption><p>The interface reviewing the specific rows selected for a migration.</p></figcaption></figure>

6. Click _Migrate Change Set_ on the toolbar along the bottom right. This will take you back to the _Migrate_ page.
7. On the _Migrate_ page, reselect the source system and integration. Then select the _Change Set Migration_ mod&#x65;_,_ select the changeset you created in step 5, then click _Migrate Change Set._

<figure><img src="../.gitbook/assets/Screenshot 2024-10-09 at 3.55.22 PM.png" alt=""><figcaption><p>Selecting a change set to migrate</p></figcaption></figure>

8. Verify that the correct changes were migrated by reviewing the migration summary.
