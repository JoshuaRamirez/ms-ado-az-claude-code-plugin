---
description: Show details of a work item by ID
allowed-tools: Bash(az boards work-item show:*)
---

# Show Work Item

Get work item details by ID.

## Usage

Ask user for the work item ID, then run:

```
az boards work-item show --id {ID} -o json
```

## Display Format

Present the key fields clearly:
- **ID**: System.Id
- **Title**: System.Title
- **Type**: System.WorkItemType
- **State**: System.State
- **Assigned To**: System.AssignedTo
- **Created By**: System.CreatedBy
- **Created Date**: System.CreatedDate
- **Area Path**: System.AreaPath
- **Iteration Path**: System.IterationPath
- **Tags**: System.Tags
- **Description**: System.Description (may contain HTML)

## Options

Show specific fields only:
```
az boards work-item show --id {ID} --fields "System.Title,System.State,System.AssignedTo"
```

Expand relations (links to other items):
```
az boards work-item show --id {ID} --expand relations
```
