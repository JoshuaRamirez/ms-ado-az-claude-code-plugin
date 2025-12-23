---
description: Create a new work item (bug, task, user story, etc.)
allowed-tools: Bash(az boards work-item create:*)
---

# Create Work Item

Create a new work item of any type.

## Required Information

Ask user for:
1. **Type**: Bug, Task, User Story, Feature, Epic, Issue, etc.
2. **Title**: Required

## Basic Creation

```
az boards work-item create \
  --type "{Type}" \
  --title "{Title}" \
  -o json
```

## With Additional Fields

```
az boards work-item create \
  --type "{Type}" \
  --title "{Title}" \
  --assigned-to "{Email or Name}" \
  --area "{Area Path}" \
  --iteration "{Iteration Path}" \
  --description "{Description}" \
  -o json
```

## Common Fields (--fields flag)

```
az boards work-item create \
  --type "Bug" \
  --title "Login button broken" \
  --fields "System.Tags=ui,critical" "Microsoft.VSTS.Common.Priority=1" "Microsoft.VSTS.Common.Severity=2 - High"
```

## Field Reference

| Field Path | Description |
|------------|-------------|
| System.AssignedTo | Person assigned |
| System.AreaPath | Area path |
| System.IterationPath | Sprint/iteration |
| System.Tags | Comma-separated tags |
| System.Description | HTML description |
| Microsoft.VSTS.Common.Priority | 1-4 |
| Microsoft.VSTS.Common.Severity | 1-4 for bugs |
| Microsoft.VSTS.Scheduling.StoryPoints | Story points |
| Microsoft.VSTS.Scheduling.Effort | Effort estimate |

## After Creation

Display the new work item ID and URL to the user.
