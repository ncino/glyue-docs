# Scheduler

The Scheduler provides a way to trigger workflow runs at specific times. This is an extremely powerful tool that gives immense configurability as to what date, time, or frequency the workflow is run at.

## Anatomy

<figure><img src="../.gitbook/assets/ETL Workflow Scheduler.png" alt=""><figcaption></figcaption></figure>

1. The name of the schedule.
2. The environment the workflow should be run in. Usually, this is set to "production".
3. A checkbox that determines whether the schedule should run or not.
4. A checkbox that enables more precise scheduling.
   1. This type of scheduling is similar to a [`cron` expression](https://en.wikipedia.org/wiki/Cron#Cron_expression).
5. The frequency at which the schedule should run.
6. The date and time the schedule should begin.
7. A calendar visualizing the configured schedule.
