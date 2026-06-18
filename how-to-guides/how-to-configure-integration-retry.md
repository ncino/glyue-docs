# How to Configure Integration Retry

Integration Retry enables automatic retry logic for failed integrations. When an integration run fails, the system can automatically retry the execution based on a configurable schedule, which reduces the need for manual intervention in cases of transient failures.

## Prerequisites

- Admin or staff permissions on the Integration Gateway instance.
- An existing integration with an Integration Config record.

## Configure Retry Settings

1. Navigate to **Build**.
2. Right-click the integration you want to configure and select **Configure**.
3. In the Configure Integration dialog, expand the **Retry Settings** panel.
4. Select a **Retry Mode**:
   - **None** — No retries (default).
   - **Fixed Interval** — Retries at a constant delay between each attempt.
   - **Exponential Backoff** — Retries with increasing delays. Each subsequent retry waits longer than the previous one.
5. Set the **Initial Retry Delay (seconds)** — the base wait time before the first retry. For Fixed Interval, this is the delay between every attempt. For Exponential Backoff, this value doubles with each subsequent attempt.
6. Set the **Retry Delay Jitter (seconds)** — a random value between 0 and this number is added to each delay. Jitter prevents multiple failing integrations from retrying at the same time.
7. Set the **Retry Count** — the maximum number of retry attempts before the integration is marked as failed.
8. Click **Save**.

## Field Reference

| Field | Type | Default | Range |
|-------|------|---------|-------|
| Retry Mode | Select | None | None, Fixed Interval, Exponential Backoff |
| Initial Retry Delay (seconds) | Integer | 5 | 1–600 |
| Retry Delay Jitter (seconds) | Integer | 2 | 0–120 |
| Retry Count | Integer | 3 | 1–10 |

## How nCino Calculates Retry Delay 

**Fixed Interval:**

```
delay = retry_delay_seconds + random(0, retry_delay_jitter)
```

**Exponential Backoff:**

```
delay = (retry_delay_seconds × 2^attempt) + random(0, retry_delay_jitter)
```

Where `attempt` starts at 0 for the first retry.

**Example:** With a 5-second base delay, 2-second jitter, and Exponential Backoff:

| Attempt | Base Delay | Possible Total (with jitter) |
|---------|-----------|------------------------------|
| 1st retry | 5s (5 × 2⁰) | 5–7s |
| 2nd retry | 10s (5 × 2¹) | 10–12s |
| 3rd retry | 20s (5 × 2²) | 20–22s |

## Viewing Retry History

Each retry attempt creates its own entry in the Run History. Retry runs are labeled with `retry_for_run_id:<original_id>`, which links them back to the original failed run.

The `retry_attempt` variable is available in integration hooks, which allows hook logic to behave differently based on the current attempt number.

## Limits

The maximum effective retry delay cannot exceed 5 minutes (300 seconds). For Exponential Backoff mode, the system validates that the longest possible delay (on the final retry attempt plus jitter) stays within this limit.
