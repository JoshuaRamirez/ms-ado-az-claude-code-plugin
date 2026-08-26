---
description: List boards for a team
allowed-tools: Bash(az devops invoke:*)
---

# List Boards

Get all boards for a team.

**Note**: Uses `az devops invoke` since CLI doesn't support boards directly.

Getting a named board via extra `board=` on `--resource boards` is not on the verified path (extra route parameters can KeyError). List boards with `--resource boards` and route-parameters `project` and `team` only. To pick a named board, filter the list JSON by `name`. Column configuration for a named board belongs on `/ado:boards:columns` (verified `--resource columns` with `board=`).

## List Boards

```
az devops invoke \
  --area work \
  --resource boards \
  --route-parameters project={PROJECT} team={TEAM} \
  --api-version 7.0 \
  -o json
```

If team is not specified, uses default team (same as project name).

## Response

Returns array of boards with:
- `id` - Board ID
- `name` - Board name (e.g., "Stories", "Epics", "Features")
- `columns` - Column definitions
- `rows` - Swimlane definitions

Typical `name` values in the list response:
- `Stories` - User stories/PBIs
- `Epics` - Epic items
- `Features` - Feature items
- `Bugs` - Bug items (if using Bugs on board)

## Pick a named board

Filter the list JSON by `name` (for example `Features`). Do not add `board=` to `--resource boards`.

For column configuration of a named board, use `/ado:boards:columns` (verified `--resource columns` with route-parameters `project`, `team`, and `board`).

## Common Teams

If user doesn't know team name, list teams first:
```
az devops team list --project {PROJECT} -o table
```
