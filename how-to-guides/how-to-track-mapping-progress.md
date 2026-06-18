# How to Track Mapping Progress

The Mapping Progress tab on the Dashboard displays field mapping completion across service requests, with forecast data, target dates, and status breakdowns for an integration.

## Mapping Progress Workflow

To review mapping progress for an integration:

1. Click **Dashboard** and select the **Mapping Progress** tab.
2. Choose an integration from the drop-down menu. If you set a default integration, it loads automatically.
3. Review the dashboard components:
   - **Overall progress bar** — Percentage of fields in a done status across all service requests, with a count of unresolved comments.
   - **Forecast cards** — Four stat cards that summarize the project trajectory. See [Configure Forecast Settings](#configure-forecast-settings).
   - **Pie chart** — Fields broken down by status category (Done, In Progress, Unstarted, and No Status).
   - **Service request list** — Each service request with its own progress bar and completion percentage. Click the arrow button next to a service request name to open its field mappings on the Mapping page.

## Configure Forecast Settings

The four forecast cards display these metrics:

- **Project Status** — Whether the integration is on track, ahead of schedule, behind schedule, or past due. The system calculates this from the target date and the configured fields-per-day rate, which only counts weekdays (Monday through Friday). If you do not set a target date, the status displays as "In Progress."
- **Target Date** — The date the integration should reach completion. If the project is behind schedule, a "Need X/day" indicator shows the daily throughput required to finish on time.
- **Fields / Day** — The configured daily mapping rate used for forecast calculations.
- **Fields Left** — How many fields remain out of the total.

Users with admin or staff permissions can configure the target date. To set a target date:

1. Click the pencil icon on the **Target Date** card.
2. Select a target date from the date picker.
3. Click the check icon or click away to save.

To set the daily mapping rate:

1. Click the pencil icon on the **Fields / Day** card.
2. Enter the expected daily mapping throughput.
3. Click away to save.

The Project Status card updates immediately to reflect whether the current rate meets the target.

## Set a Default Integration

To set a default integration that loads automatically when you open the Mapping Progress tab:

1. Select the integration from the drop-down menu.
2. Click **Set as default** below the drop-down menu.

The star icon fills in to confirm. The default persists across sessions. To change the default, select a different integration and click **Set as default** again.
