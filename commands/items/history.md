---
description: Show work item revision history
allowed-tools: Bash(az devops invoke:*), Bash(az boards work-item show:*)
---

# Work Item History

View the complete revision history of a work item, showing what changed, when, and by whom.

## Usage

Ask user for the work item ID, then retrieve the revision history.

## Step 1: Get Revisions

```bash
az devops invoke \
  --area wit \
  --resource revisions \
  --route-parameters project={PROJECT} id={ID} \
  --api-version 7.1 \
  -o json
```

**Note**: The `project` parameter should match your configured project. You can check it with `az devops configure --list`.

## Response Format

The API returns an array of revisions, each containing:
- `rev` - Revision number (1 is the original creation)
- `fields.System.ChangedDate` - When the change was made
- `fields.System.ChangedBy` - Who made the change
- All field values as they were at that revision

## Step 2: Display History

Present the revision history in a clear format:

### Suggested Output Format

```
Work Item #{ID} - Revision History
==================================

Revision #3 | 2024-01-15 14:30:00 | Changed by: Jane Smith
  - State: Active -> Resolved
  - AssignedTo: John Doe -> Jane Smith

Revision #2 | 2024-01-14 10:15:00 | Changed by: John Doe
  - Priority: 2 -> 1
  - Tags: (none) -> urgent,production

Revision #1 | 2024-01-13 09:00:00 | Created by: John Doe
  - Title: Fix login button
  - Type: Bug
  - State: New
  - Priority: 2
```

## Comparing Revisions

To identify what changed between revisions, compare the field values across consecutive revisions. Key fields to track:

| Field | Description |
|-------|-------------|
| System.State | Work item state transitions |
| System.AssignedTo | Assignment changes |
| System.Title | Title edits |
| System.Description | Description updates |
| System.Tags | Tag additions/removals |
| System.IterationPath | Sprint/iteration changes |
| System.AreaPath | Area path changes |
| Microsoft.VSTS.Common.Priority | Priority changes |
| Microsoft.VSTS.Common.Severity | Severity changes (bugs) |
| Microsoft.VSTS.Scheduling.StoryPoints | Story point adjustments |

## Getting a Specific Revision

To view the work item as it was at a specific revision:

```bash
az devops invoke \
  --area wit \
  --resource revisions \
  --route-parameters project={PROJECT} id={ID} revisionNumber={REV} \
  --api-version 7.1 \
  -o json
```

## Getting Current Revision Number

To find the current (latest) revision number, use the standard show command:

```bash
az boards work-item show --id {ID} --fields "System.Rev" -o json
```

The `rev` field in the response indicates the current revision number.

## Common Use Cases

### View Recent Changes
Show only the last N revisions by examining the returned array.

### Track State Transitions
Filter changes to focus on `System.State` field changes to see the workflow history.

### Find Who Made a Specific Change
Look through revisions to identify when and who changed a particular field.

### Audit Trail
Use revision history for compliance or debugging to understand how an item evolved.

## Notes

- Revision 1 is always the creation event
- Each revision captures the complete state of the work item at that point
- Comments are not included in revisions - use the comments command for discussion history
- Large work items with many revisions may return a significant amount of data
