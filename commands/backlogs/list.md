---
description: List backlogs for a team
allowed-tools: Bash(az devops invoke:*)
---

# List Backlogs

Get all backlog levels for a team.

## Get Backlogs

```
az devops invoke \
  --area work \
  --resource backlogs \
  --route-parameters project={PROJECT} team={TEAM} \
  --api-version 7.0 \
  -o json
```

## Response Format

Returns array of backlog levels:
```json
{
  "value": [
    {
      "id": "Microsoft.EpicCategory",
      "name": "Epics",
      "rank": 1,
      "workItemTypes": [{"name": "Epic"}]
    },
    {
      "id": "Microsoft.FeatureCategory",
      "name": "Features",
      "rank": 2,
      "workItemTypes": [{"name": "Feature"}]
    },
    {
      "id": "Microsoft.RequirementCategory",
      "name": "Stories",
      "rank": 3,
      "workItemTypes": [{"name": "User Story", "name": "Bug"}]
    }
  ]
}
```

Key fields:
- `id` - Backlog category ID
- `name` - Display name
- `rank` - Order (1 = highest level)
- `workItemTypes` - Work item types in this backlog
