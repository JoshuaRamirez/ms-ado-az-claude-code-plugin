---
description: View and manage board columns
allowed-tools: Bash(az devops invoke:*), Bash(echo:*), Write
---

# Board Columns

View and update column settings on a board.

## Get Board Columns

```
az devops invoke \
  --area work \
  --resource boardcolumns \
  --route-parameters project={PROJECT} team={TEAM} board={BOARD} \
  --api-version 7.1 \
  -o json
```

## Response Format

Returns array of columns:
```json
{
  "value": [
    {
      "id": "...",
      "name": "New",
      "itemLimit": 0,
      "stateMappings": {"Bug": "New", "User Story": "New"},
      "columnType": "incoming"
    },
    {
      "id": "...",
      "name": "Active",
      "itemLimit": 5,
      "isSplit": true,
      "stateMappings": {"Bug": "Active", "User Story": "Active"},
      "columnType": "inProgress"
    }
  ]
}
```

Key fields:
- `name` - Column display name
- `itemLimit` - WIP limit (0 = no limit)
- `isSplit` - Has Doing/Done split
- `stateMappings` - Maps work item types to states
- `columnType` - incoming, inProgress, outgoing

## Update Board Columns

### Step 1: Get current columns and modify

### Step 2: Create update JSON with full column array

```json
{
  "value": [
    {"name": "New", "itemLimit": 0, "columnType": "incoming", "stateMappings": {"User Story": "New"}},
    {"name": "Active", "itemLimit": 5, "isSplit": true, "columnType": "inProgress", "stateMappings": {"User Story": "Active"}},
    {"name": "Resolved", "itemLimit": 3, "columnType": "inProgress", "stateMappings": {"User Story": "Resolved"}},
    {"name": "Closed", "itemLimit": 0, "columnType": "outgoing", "stateMappings": {"User Story": "Closed"}}
  ]
}
```

### Step 3: Apply update

```
az devops invoke \
  --area work \
  --resource boardcolumns \
  --route-parameters project={PROJECT} team={TEAM} board={BOARD} \
  --http-method PUT \
  --in-file columns.json \
  --api-version 7.1 \
  -o json
```

## Notes

- Updates replace ALL columns - must include complete column list
- Column order matters - displayed left to right
- WIP limits are per column, not enforced by system (just visual)
