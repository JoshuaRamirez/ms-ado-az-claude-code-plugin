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

## Common Updates

### Change State
```
az boards work-item update --id {ID} --state "Resolved"
az boards work-item update --id {ID} --state "Closed"
```

### Reassign
```
az boards work-item update --id {ID} --assigned-to "newowner@example.com"
```

### Move to Different Sprint
```
az boards work-item update --id {ID} --iteration "Project\\Sprint 6"
```

### Move to Different Area
```
az boards work-item update --id {ID} --area "Project\\Team A"
```

### Add/Update Description
```
az boards work-item update --id {ID} --description "<p>Updated description</p>"
```

## Notes

- Field values with spaces need quotes
- Iteration/Area paths use backslash: `Project\\Team\\SubArea`
- Some state transitions may be restricted by process rules
