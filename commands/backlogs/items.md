---
description: Get work items from a backlog
allowed-tools: Bash(az devops invoke:*)
---

# Backlog Work Items

Get work items in a specific backlog level.

## Get Backlog Items

```
az devops invoke \
  --area work \
  --resource backlogs \
  --route-parameters project={PROJECT} team={TEAM} backlogId={BACKLOG_ID} \
  --resource workitems \
  --api-version 7.1 \
  -o json
```

Common backlog IDs:
- `Microsoft.EpicCategory` - Epics
- `Microsoft.FeatureCategory` - Features
- `Microsoft.RequirementCategory` - Stories/PBIs

## Example: Get Stories Backlog

```
az devops invoke \
  --area work \
  --resource backlogs \
  --route-parameters project={PROJECT} team={TEAM} backlogId=Microsoft.RequirementCategory \
  --query-parameters "\$top=50" \
  --api-version 7.1 \
  -o json
```

## Response Format

Returns work item references with:
- `target.id` - Work item ID
- `target.url` - API URL to get full item

To get full details, use the IDs with `az boards work-item show` or batch get.

## Get Items with Order

The response includes `rel` field showing parent relationships if items are nested.

## Pagination

Use query parameters:
- `$top` - Number of items to return
- `$skip` - Number to skip (for paging)

## Alternative: WIQL Query

For more control, use WIQL:
```
az boards query --wiql "SELECT [System.Id], [System.Title], [Microsoft.VSTS.Common.BacklogPriority] FROM WorkItems WHERE [System.WorkItemType] = 'User Story' AND [System.AreaPath] UNDER '{TEAM_AREA}' ORDER BY [Microsoft.VSTS.Common.BacklogPriority]"
```
