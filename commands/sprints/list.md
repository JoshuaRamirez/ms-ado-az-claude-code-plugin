---
description: List iterations/sprints for a team
allowed-tools: Bash(az boards iteration:*)
---

# List Sprints/Iterations

View iterations assigned to a team.

## List Team Iterations

```
az boards iteration team list --team {TEAM} -o table
```

Or with JSON output for more detail:
```
az boards iteration team list --team {TEAM} -o json
```

## Response Fields

- `id` - Iteration GUID
- `name` - Iteration name (e.g., "Sprint 5")
- `path` - Full path (e.g., "Project\\Sprint 5")
- `attributes.startDate` - Sprint start
- `attributes.finishDate` - Sprint end
- `attributes.timeFrame` - past, current, or future

## Show Current Iteration

Filter to current:
```
az boards iteration team list --team {TEAM} --timeframe current -o json
```

## Show Iteration Details

```
az boards iteration team show --team {TEAM} --id {ITERATION_ID} -o json
```

## List All Project Iterations

To see all iterations (not just team-assigned):
```
az boards iteration project list -o table
```

## Add Iteration to Team

```
az boards iteration team add --team {TEAM} --id {ITERATION_ID}
```

## Remove Iteration from Team

```
az boards iteration team remove --team {TEAM} --id {ITERATION_ID}
```

## Set Default Iteration

```
az boards iteration team set-default-iteration --team {TEAM} --id {ITERATION_ID}
```

## Set Backlog Iteration

The backlog iteration is the root for the team's backlog:
```
az boards iteration team set-backlog-iteration --team {TEAM} --id {ITERATION_ID}
```
