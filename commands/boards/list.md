---
description: List boards for a team
allowed-tools: Bash(az devops invoke:*)
---

# List Boards

Get all boards for a team.

**Note**: Uses `az devops invoke` since CLI doesn't support boards directly.

## List Boards

```
az devops invoke \
  --area work \
  --resource boards \
  --route-parameters project={PROJECT} team={TEAM} \
  --api-version 7.1 \
  -o json
```

If team is not specified, uses default team (same as project name).

## Response

Returns array of boards with:
- `id` - Board ID
- `name` - Board name (e.g., "Stories", "Epics", "Features")
- `columns` - Column definitions
- `rows` - Swimlane definitions

## Get Specific Board

```
az devops invoke \
  --area work \
  --resource boards \
  --route-parameters project={PROJECT} team={TEAM} board={BOARD_NAME} \
  --api-version 7.1 \
  -o json
```

Board names are typically:
- `Stories` - User stories/PBIs
- `Epics` - Epic items
- `Features` - Feature items
- `Bugs` - Bug items (if using Bugs on board)

## Common Teams

If user doesn't know team name, list teams first:
```
az devops team list --project {PROJECT} -o table
```
