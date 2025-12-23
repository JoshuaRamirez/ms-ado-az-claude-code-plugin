---
description: Query work items using WIQL (Work Item Query Language)
allowed-tools: Bash(az boards query:*)
---

# Query Work Items

Execute WIQL queries to find work items.

## Basic Query

```
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State] FROM WorkItems WHERE [System.State] = 'Active'" -o json
```

## Common Queries

### My Active Items
```
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State], [System.WorkItemType] FROM WorkItems WHERE [System.AssignedTo] = @Me AND [System.State] <> 'Closed' ORDER BY [System.ChangedDate] DESC" -o json
```

### Items in Current Sprint
```
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State] FROM WorkItems WHERE [System.IterationPath] = @CurrentIteration AND [System.TeamProject] = @Project" -o json
```

### Bugs by Priority
```
az boards query --wiql "SELECT [System.Id], [System.Title], [Microsoft.VSTS.Common.Priority] FROM WorkItems WHERE [System.WorkItemType] = 'Bug' AND [System.State] <> 'Closed' ORDER BY [Microsoft.VSTS.Common.Priority]" -o json
```

### Recently Changed
```
az boards query --wiql "SELECT [System.Id], [System.Title], [System.ChangedDate] FROM WorkItems WHERE [System.ChangedDate] >= @Today - 7 ORDER BY [System.ChangedDate] DESC" -o json
```

### Items Under Area Path
```
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.AreaPath] UNDER 'Project\\Team A'" -o json
```

### Items with Specific Tag
```
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.Tags] CONTAINS 'urgent'" -o json
```

## WIQL Reference

### Operators
- `=`, `<>`, `>`, `<`, `>=`, `<=`
- `CONTAINS`, `NOT CONTAINS`
- `IN`, `NOT IN`
- `UNDER` (for hierarchical paths)
- `AND`, `OR`

### Macros
- `@Me` - Current user
- `@Today` - Today's date
- `@CurrentIteration` - Current sprint
- `@Project` - Current project

### Fields
- `[System.Id]`, `[System.Title]`, `[System.State]`
- `[System.AssignedTo]`, `[System.CreatedBy]`
- `[System.WorkItemType]`
- `[System.AreaPath]`, `[System.IterationPath]`
- `[System.Tags]`
- `[System.CreatedDate]`, `[System.ChangedDate]`

## Run Saved Query by ID

```
az boards query --id {QUERY_GUID} -o json
```
