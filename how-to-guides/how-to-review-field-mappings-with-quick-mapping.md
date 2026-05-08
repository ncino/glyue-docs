# How to Review Field Mappings with Quick Mapping

Quick Mapping lets you review a service request's unmapped fields one at a time through a card-based interface. Mark each field as approved, not used, or change requested — no need to scroll through the full table.

Quick Mapping automatically skips fields already marked as Approved, Not Used, or Change Requested. It only shows fields that still need attention.

## Quick Mapping Workflow

To review unmapped fields for a service request:

1. Click **Mapping** and select an integration from the drop-down menu.
2. Select the service request you want to review.
3. Click **Quick Mapping** above the field mapping table.
4. For each field, decide if you need to map the field:
   - **Yes** — Continue to the value check. See [Approve or Request a Change](#approve-or-request-a-change).
   - **No** — The system marks the field as Not Used and displays the next field.
   - **Save for later** — Skip the field. Its status does not change.
5. After you review all fields, the system displays a summary of the number of fields saved and skipped.

## Approve or Request a Change

When you mark a field as needed, the card displays its current value and source information.

1. Review the current value on the card.
2. Choose an action:
   - Click **Yes** if the value is correct. The system marks the field as Approved.
   - Click **No** to describe a required change.
3. If you clicked **No**, enter a description of the correct value or source in the text area.
4. Click **Save & Continue**. The system posts a comment on the field and sets the status to Change Requested.

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **← / →** | Highlight Yes or No. Press the same arrow again to confirm. |
| **Enter** | Confirm the highlighted choice |
| **S** | Skip the current field |
| **Escape** | Close the dialog |

## How It Works

- Skipped fields remain in their original state and appear again the next time you start Quick Mapping for the same service request.
- Progress updates immediately in the service request's completion percentage and the overall integration progress bar.
