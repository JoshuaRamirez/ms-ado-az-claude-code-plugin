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

### My Work

```bash
# My active items (all types)
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State], [System.WorkItemType] FROM WorkItems WHERE [System.AssignedTo] = @Me AND [System.State] <> 'Closed' ORDER BY [System.ChangedDate] DESC" -o json

# My tasks for current sprint
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State] FROM WorkItems WHERE [System.AssignedTo] = @Me AND [System.WorkItemType] = 'Task' AND [System.IterationPath] = @CurrentIteration" -o json

# Items I created
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State] FROM WorkItems WHERE [System.CreatedBy] = @Me ORDER BY [System.CreatedDate] DESC" -o json

# Items I changed recently
az boards query --wiql "SELECT [System.Id], [System.Title], [System.ChangedDate] FROM WorkItems WHERE [System.ChangedBy] = @Me AND [System.ChangedDate] >= @Today - 7 ORDER BY [System.ChangedDate] DESC" -o json
```

### Sprint and Iteration

```bash
# All items in current sprint
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State], [System.AssignedTo] FROM WorkItems WHERE [System.IterationPath] = @CurrentIteration ORDER BY [System.State]" -o json

# Items in a specific sprint
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State] FROM WorkItems WHERE [System.IterationPath] = 'MyProject\\Sprint 5'" -o json

# Items under an iteration (includes sub-iterations)
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.IterationPath] UNDER 'MyProject\\Release 2'" -o json

# Unassigned items in current sprint
az boards query --wiql "SELECT [System.Id], [System.Title], [System.WorkItemType] FROM WorkItems WHERE [System.IterationPath] = @CurrentIteration AND [System.AssignedTo] = ''" -o json

# Incomplete items in current sprint
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State], [System.AssignedTo] FROM WorkItems WHERE [System.IterationPath] = @CurrentIteration AND [System.State] NOT IN ('Done', 'Closed', 'Removed')" -o json
```

### Bug Tracking

```bash
# All active bugs
az boards query --wiql "SELECT [System.Id], [System.Title], [Microsoft.VSTS.Common.Priority], [Microsoft.VSTS.Common.Severity] FROM WorkItems WHERE [System.WorkItemType] = 'Bug' AND [System.State] <> 'Closed' ORDER BY [Microsoft.VSTS.Common.Priority]" -o json

# Critical bugs (Priority 1)
az boards query --wiql "SELECT [System.Id], [System.Title], [System.AssignedTo] FROM WorkItems WHERE [System.WorkItemType] = 'Bug' AND [Microsoft.VSTS.Common.Priority] = 1 AND [System.State] <> 'Closed'" -o json

# Bugs by severity
az boards query --wiql "SELECT [System.Id], [System.Title], [Microsoft.VSTS.Common.Severity] FROM WorkItems WHERE [System.WorkItemType] = 'Bug' AND [Microsoft.VSTS.Common.Severity] = '1 - Critical' AND [System.State] <> 'Closed'" -o json

# Bugs without repro steps
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.WorkItemType] = 'Bug' AND [Microsoft.VSTS.TCM.ReproSteps] = '' AND [System.State] <> 'Closed'" -o json

# Old unresolved bugs (created more than 30 days ago)
az boards query --wiql "SELECT [System.Id], [System.Title], [System.CreatedDate] FROM WorkItems WHERE [System.WorkItemType] = 'Bug' AND [System.State] <> 'Closed' AND [System.CreatedDate] < @Today - 30 ORDER BY [System.CreatedDate]" -o json
```

### Backlog and Features

```bash
# User stories in backlog (not in any sprint)
az boards query --wiql "SELECT [System.Id], [System.Title], [Microsoft.VSTS.Scheduling.StoryPoints] FROM WorkItems WHERE [System.WorkItemType] = 'User Story' AND [System.IterationPath] = 'MyProject' AND [System.State] <> 'Closed'" -o json

# Features without target date
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.WorkItemType] = 'Feature' AND [Microsoft.VSTS.Scheduling.TargetDate] = '' AND [System.State] <> 'Closed'" -o json

# Epics in progress
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State] FROM WorkItems WHERE [System.WorkItemType] = 'Epic' AND [System.State] = 'In Progress'" -o json

# User stories without story points
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.WorkItemType] = 'User Story' AND [Microsoft.VSTS.Scheduling.StoryPoints] = '' AND [System.State] <> 'Closed'" -o json
```

### Team and Area

```bash
# Items in a team's area
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State] FROM WorkItems WHERE [System.AreaPath] = 'MyProject\\Team Alpha'" -o json

# Items under an area (includes sub-areas)
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.AreaPath] UNDER 'MyProject\\Backend'" -o json

# Items assigned to team members
az boards query --wiql "SELECT [System.Id], [System.Title], [System.AssignedTo] FROM WorkItems WHERE [System.AssignedTo] IN ('user1@example.com', 'user2@example.com') AND [System.State] <> 'Closed'" -o json
```

### Tags and Search

```bash
# Items with specific tag
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.Tags] CONTAINS 'urgent'" -o json

# Items with multiple tags (AND)
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.Tags] CONTAINS 'urgent' AND [System.Tags] CONTAINS 'production'" -o json

# Items with any of multiple tags (OR)
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.Tags] CONTAINS 'urgent' OR [System.Tags] CONTAINS 'blocking'" -o json

# Title contains keyword
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.Title] CONTAINS 'login'" -o json
```

### Date-Based Queries

```bash
# Changed today
az boards query --wiql "SELECT [System.Id], [System.Title], [System.ChangedBy] FROM WorkItems WHERE [System.ChangedDate] >= @Today ORDER BY [System.ChangedDate] DESC" -o json

# Changed this week
az boards query --wiql "SELECT [System.Id], [System.Title], [System.ChangedDate] FROM WorkItems WHERE [System.ChangedDate] >= @Today - 7 ORDER BY [System.ChangedDate] DESC" -o json

# Created in last 30 days
az boards query --wiql "SELECT [System.Id], [System.Title], [System.CreatedDate] FROM WorkItems WHERE [System.CreatedDate] >= @Today - 30 ORDER BY [System.CreatedDate] DESC" -o json

# Items with target date in next 2 weeks
az boards query --wiql "SELECT [System.Id], [System.Title], [Microsoft.VSTS.Scheduling.TargetDate] FROM WorkItems WHERE [Microsoft.VSTS.Scheduling.TargetDate] >= @Today AND [Microsoft.VSTS.Scheduling.TargetDate] <= @Today + 14 ORDER BY [Microsoft.VSTS.Scheduling.TargetDate]" -o json

# Overdue items (target date passed)
az boards query --wiql "SELECT [System.Id], [System.Title], [Microsoft.VSTS.Scheduling.TargetDate] FROM WorkItems WHERE [Microsoft.VSTS.Scheduling.TargetDate] < @Today AND [System.State] <> 'Closed'" -o json
```

### State and Status

```bash
# Items by state
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.State] = 'Active'" -o json

# Items in multiple states
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State] FROM WorkItems WHERE [System.State] IN ('New', 'Active', 'In Progress')" -o json

# Blocked items
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.Tags] CONTAINS 'blocked'" -o json

# Recently resolved
az boards query --wiql "SELECT [System.Id], [System.Title], [System.ChangedDate] FROM WorkItems WHERE [System.State] = 'Resolved' AND [System.ChangedDate] >= @Today - 7 ORDER BY [System.ChangedDate] DESC" -o json
```

### Parent-Child Relationships

```bash
# Items with a specific parent
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.Parent] = 1234" -o json

# Items with any of several parents
az boards query --wiql "SELECT [System.Id], [System.Title], [System.Parent] FROM WorkItems WHERE [System.Parent] IN (1234, 1235, 1236)" -o json

# Items without a parent
az boards query --wiql "SELECT [System.Id], [System.Title] FROM WorkItems WHERE [System.Parent] = '' AND [System.WorkItemType] IN ('User Story', 'Bug')" -o json
```

## WIQL Reference

### Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `=` | Equals | `[System.State] = 'Active'` |
| `<>` | Not equals | `[System.State] <> 'Closed'` |
| `>`, `<`, `>=`, `<=` | Comparison | `[Microsoft.VSTS.Common.Priority] <= 2` |
| `IN` | In list | `[System.State] IN ('New', 'Active')` |
| `NOT IN` | Not in list | `[System.State] NOT IN ('Closed', 'Removed')` |
| `CONTAINS` | Contains text | `[System.Tags] CONTAINS 'urgent'` |
| `NOT CONTAINS` | Does not contain | `[System.Title] NOT CONTAINS 'test'` |
| `UNDER` | Under hierarchy | `[System.AreaPath] UNDER 'Project\\Team'` |
| `AND`, `OR` | Logical operators | Multiple conditions |

### Macros

| Macro | Description |
|-------|-------------|
| `@Me` | Current user |
| `@Today` | Today's date at midnight |
| `@Today - N` | N days ago |
| `@Today + N` | N days from now |
| `@CurrentIteration` | Team's current sprint |
| `@Project` | Current project (use explicit name if unreliable) |

### Common Fields

| Field | Description |
|-------|-------------|
| `[System.Id]` | Work item ID |
| `[System.Title]` | Title |
| `[System.State]` | Current state |
| `[System.WorkItemType]` | Type (Bug, Task, etc.) |
| `[System.AssignedTo]` | Assigned person |
| `[System.CreatedBy]` | Creator |
| `[System.CreatedDate]` | Creation date |
| `[System.ChangedBy]` | Last modifier |
| `[System.ChangedDate]` | Last change date |
| `[System.AreaPath]` | Area path |
| `[System.IterationPath]` | Iteration/sprint path |
| `[System.Tags]` | Tags |
| `[System.Parent]` | Parent work item ID |
| `[Microsoft.VSTS.Common.Priority]` | Priority (1-4) |
| `[Microsoft.VSTS.Common.Severity]` | Severity (bugs) |
| `[Microsoft.VSTS.Scheduling.StoryPoints]` | Story points |
| `[Microsoft.VSTS.Scheduling.TargetDate]` | Target date |

## Run Saved Query

Execute a saved query by its GUID:
```bash
az boards query --id {QUERY_GUID} -o json
```

## WIQL Limitations

**Not Supported:**
- `LIKE` operator - Use `CONTAINS` instead
- `ORDER BY [System.Parent]` - Cannot sort by parent
- `TOP N` clause - Use `--query` with JMESPath slice syntax

**Workarounds:**
```bash
# Instead of LIKE, use CONTAINS:
WHERE [System.Title] CONTAINS 'keyword'

# Instead of TOP N, use JMESPath slice:
az boards query --wiql "..." --query "[:10]" -o json
```

## Tips

1. Always include `ORDER BY` for consistent results
2. Use `[System.State] <> 'Closed'` to exclude closed items
3. Quote string values: `'value with spaces'`
4. Field names are case-insensitive but must have brackets
5. Use explicit project name if `@Project` macro is unreliable
