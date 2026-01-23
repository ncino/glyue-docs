# Run History

The Run History page gives a general overview of past (and current) workflow runs, and enables one to view details about individual workflow runs.

## Anatomy

<figure><img src="../.gitbook/assets/ETL Workflow Runs.png" alt=""><figcaption></figcaption></figure>

1. The ID of the workflow run.
2. The name of the workflow that ran, along with which SDLC-phase it ran as.
3. A filter button.
4. The status of the workflow run (i.e., Completed, Failed, Running).
5. The date and time the workflow run started.
6. The duration of the workflow run (if it finished).
7. Any errors that occurred during execution, and a button to see further step-by-step details.

## Run Details

In addition to a general overview of all workflow runs that occurred, you can view details about individual workflow runs. This includes how many records were processed by each task and any errors that occurred during execution. This is extremely useful for debugging a failing workflow.

<figure><img src="../.gitbook/assets/ETL Workflow Run Details (Success).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/ETL Workflow Run Details (Failed).png" alt=""><figcaption></figcaption></figure>
