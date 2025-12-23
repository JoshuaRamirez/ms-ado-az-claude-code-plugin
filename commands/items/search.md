---
description: Full-text search for work items
allowed-tools: Bash(az devops invoke:*), Bash(echo:*), Write
---

# Search Work Items

Full-text search across work item fields using the Search API.

**Note**: This uses `az devops invoke` since CLI doesn't support search.

## Step 1: Create Search Request

Create a JSON file with the search parameters:

```json
{
  "searchText": "{user's search query}",
  "$top": 25,
  "$skip": 0,
  "filters": {
    "System.TeamProject": ["{PROJECT}"]
  }
}
```

Save to a temp file, e.g., `search_request.json`

## Step 2: Execute Search

```
az devops invoke \
  --area search \
  --resource workItemSearchResults \
  --http-method POST \
  --in-file search_request.json \
  --api-version 7.1-preview.1 \
  -o json
```

## Response Format

Results are in `results` array with:
- `fields.system.id` - Work item ID
- `fields.system.title` - Title
- `fields.system.state` - State
- `fields.system.workitemtype` - Type
- `hits` - Matching snippets with highlights

## Advanced Filters

Filter by work item type:
```json
{
  "searchText": "login error",
  "$top": 25,
  "filters": {
    "System.TeamProject": ["MyProject"],
    "System.WorkItemType": ["Bug", "Task"]
  }
}
```

Filter by state:
```json
{
  "searchText": "performance",
  "$top": 25,
  "filters": {
    "System.TeamProject": ["MyProject"],
    "System.State": ["Active", "New"]
  }
}
```

Filter by assigned to:
```json
{
  "searchText": "api",
  "$top": 25,
  "filters": {
    "System.TeamProject": ["MyProject"],
    "System.AssignedTo": ["user@example.com"]
  }
}
```

## Clean Up

Delete the temp JSON file after search completes.
