---
description: Get work items from a backlog level using WIQL
allowed-tools: Bash(az boards query:*), Bash(az devops invoke:*)
---

# Backlog Work Items

Get work items in a specific backlog level.

Getting backlog items via `az devops invoke` `--resource workitems`, or extra `backlogId` on `--resource backlogs`, is not on the verified path (unverified resources and extra route parameters can KeyError). Use WIQL (`az boards query`) to get work items. Discover backlog levels with `/ado:backlogs:list` or the verified Get Backlogs invoke below.

## Discover backlog levels

```
az devops invoke \
  --area work \
  --resource backlogs \
  --route-parameters project={PROJECT} team={TEAM} \
  --api-version 7.0 \
  -o json
```

Returns backlog levels with `id`, `name`, `rank`, and `workItemTypes`. Use those types in WIQL. Do not pass `backlogId` as an invoke route parameter.

Common work item types by backlog level:
- Epics: `Epic`
- Features: `Feature`
- Stories/PBIs: `User Story` (or `Product Backlog Item`; may include `Bug`)

## Get work items in a backlog level (WIQL)

Filter by work item type and team area (and iteration when needed):

```
az boards query --wiql "SELECT [System.Id], [System.Title], [Microsoft.VSTS.Common.BacklogPriority] FROM WorkItems WHERE [System.WorkItemType] = 'User Story' AND [System.AreaPath] UNDER '{TEAM_AREA}' ORDER BY [Microsoft.VSTS.Common.BacklogPriority]" -o json
```

## Example: Stories in an iteration

```
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State], [Microsoft.VSTS.Common.BacklogPriority] FROM WorkItems WHERE [System.WorkItemType] IN ('User Story', 'Bug') AND [System.AreaPath] UNDER '{TEAM_AREA}' AND [System.IterationPath] UNDER '{ITERATION}' ORDER BY [Microsoft.VSTS.Common.BacklogPriority]" -o json
```

To get full details, use the IDs with `az boards work-item show`.
