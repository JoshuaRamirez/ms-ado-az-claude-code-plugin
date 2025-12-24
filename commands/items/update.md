---
description: Update an existing work item
allowed-tools: Bash(az boards work-item update:*)
---

# Update Work Item

Modify fields on an existing work item.

## Required Information

1. **Work Item ID**: Required
2. **Fields to update**: At least one

## Basic Update

Update a single field:
```
az boards work-item update \
  --id {ID} \
  --state "Active" \
  -o json
```

## Update Multiple Fields

```
az boards work-item update \
  --id {ID} \
  --title "Updated title" \
  --assigned-to "user@example.com" \
  --iteration "Project\\Sprint 5" \
  -o json
```

## Using --fields for Any Field

```
az boards work-item update \
  --id {ID} \
  --fields "System.Tags=urgent,reviewed" "Microsoft.VSTS.Common.Priority=1"
```

## Common Update Scenarios

### State Transitions

Move through the workflow:
```bash
# Start working on an item
az boards work-item update --id {ID} --state "Active" -o json

# Complete code review
az boards work-item update --id {ID} --state "Resolved" -o json

# Close as done
az boards work-item update --id {ID} --state "Closed" -o json

# Reopen a closed item
az boards work-item update --id {ID} --state "Active" -o json
```

### Assignment Changes

```bash
# Assign to someone
az boards work-item update --id {ID} --assigned-to "developer@example.com" -o json

# Reassign to a different person
az boards work-item update --id {ID} --assigned-to "newowner@example.com" -o json

# Unassign (clear assignment)
az boards work-item update --id {ID} --assigned-to "" -o json
```

### Sprint and Iteration Management

```bash
# Move to a specific sprint
az boards work-item update --id {ID} --iteration "MyProject\\Sprint 6" -o json

# Move to backlog (root iteration)
az boards work-item update --id {ID} --iteration "MyProject" -o json

# Move to a release iteration
az boards work-item update --id {ID} --iteration "MyProject\\Release 2\\Sprint 1" -o json
```

### Area Path Changes

```bash
# Move to a team's area
az boards work-item update --id {ID} --area "MyProject\\Team Alpha" -o json

# Move to a component area
az boards work-item update --id {ID} --area "MyProject\\Backend\\API" -o json
```

### Priority and Severity

```bash
# Set high priority
az boards work-item update --id {ID} \
  --fields "Microsoft.VSTS.Common.Priority=1" -o json

# Update bug severity
az boards work-item update --id {ID} \
  --fields "Microsoft.VSTS.Common.Severity=2 - High" -o json

# Set both priority and severity
az boards work-item update --id {ID} \
  --fields "Microsoft.VSTS.Common.Priority=1" \
           "Microsoft.VSTS.Common.Severity=1 - Critical" -o json
```

### Tags Management

```bash
# Replace all tags
az boards work-item update --id {ID} \
  --fields "System.Tags=urgent,reviewed,production" -o json

# Clear all tags
az boards work-item update --id {ID} \
  --fields "System.Tags=" -o json
```

**Note**: Tags are comma-separated. The update replaces existing tags entirely.

### Description and Content

```bash
# Update description
az boards work-item update --id {ID} \
  --description "<p>Updated description with <b>formatting</b>.</p>" -o json

# Update bug repro steps
az boards work-item update --id {ID} \
  --fields "Microsoft.VSTS.TCM.ReproSteps=<ol><li>Step 1</li><li>Step 2</li><li>Step 3</li></ol>" -o json

# Update acceptance criteria
az boards work-item update --id {ID} \
  --fields "Microsoft.VSTS.Common.AcceptanceCriteria=<ul><li>Criteria 1</li><li>Criteria 2</li></ul>" -o json
```

### Effort and Time Tracking

```bash
# Update story points
az boards work-item update --id {ID} \
  --fields "Microsoft.VSTS.Scheduling.StoryPoints=8" -o json

# Update task hours
az boards work-item update --id {ID} \
  --fields "Microsoft.VSTS.Scheduling.RemainingWork=2" \
           "Microsoft.VSTS.Scheduling.CompletedWork=6" -o json

# Update original estimate
az boards work-item update --id {ID} \
  --fields "Microsoft.VSTS.Scheduling.OriginalEstimate=8" -o json
```

### Target Dates

```bash
# Set target date for feature
az boards work-item update --id {ID} \
  --fields "Microsoft.VSTS.Scheduling.TargetDate=2024-06-30" -o json

# Set start and target dates
az boards work-item update --id {ID} \
  --fields "Microsoft.VSTS.Scheduling.StartDate=2024-01-15" \
           "Microsoft.VSTS.Scheduling.TargetDate=2024-03-31" -o json
```

### Adding Comments

```bash
# Add a discussion comment while updating
az boards work-item update --id {ID} \
  --discussion "Reviewed and approved. Ready for development." -o json

# Combine comment with state change
az boards work-item update --id {ID} \
  --state "Resolved" \
  --discussion "Fixed in commit abc123. Please verify." -o json
```

### Complex Updates

```bash
# Complete sprint planning for a task
az boards work-item update --id {ID} \
  --state "Active" \
  --assigned-to "developer@example.com" \
  --iteration "MyProject\\Sprint 5" \
  --fields "Microsoft.VSTS.Scheduling.RemainingWork=4" \
           "Microsoft.VSTS.Common.Activity=Development" \
  --discussion "Assigned during sprint planning" \
  -o json

# Triage a bug
az boards work-item update --id {ID} \
  --state "Active" \
  --assigned-to "developer@example.com" \
  --fields "Microsoft.VSTS.Common.Priority=2" \
           "Microsoft.VSTS.Common.Severity=3 - Medium" \
           "System.Tags=triaged,sprint-5" \
  --discussion "Triaged: P2/Medium severity. Target Sprint 5." \
  -o json
```

## Bulk Updates

Update multiple items efficiently:
```bash
# Update state for multiple items
for id in 1001 1002 1003; do
  az boards work-item update --id $id --state "Active" -o none && echo "Updated $id"
done

# Move multiple items to a sprint
for id in 1001 1002 1003; do
  az boards work-item update --id $id --iteration "MyProject\\Sprint 5" -o none
done
```

Use `-o none` to suppress JSON output for faster execution.

## Notes

- Field values with spaces need quotes
- Iteration/Area paths use backslash as separator: `Project\\Team\\SubArea`
- Some state transitions may be restricted by process rules
- Invalid field names or values will cause the command to fail
- The `--discussion` flag adds a comment while updating other fields
- Date fields expect YYYY-MM-DD format

## Troubleshooting

### "Field cannot be set" Error
Some fields are read-only or calculated. Check the field's rules in the process template.

### "Invalid state transition" Error
Not all state transitions are allowed. Check the work item type's workflow definition.

### Path Not Found
Verify area/iteration paths exist using:
```bash
az boards area project list -o table
az boards iteration project list -o table
```
