---
description: Manage team days off for a sprint
allowed-tools: Bash(az devops invoke:*), Bash(echo:*), Write
---

# Team Days Off

Set team-wide days off (holidays, team events) for an iteration.

## Get Team Days Off

```
az devops invoke \
  --area work \
  --resource teamdaysoff \
  --route-parameters project={PROJECT} team={TEAM} iterationId={ITERATION_ID} \
  --api-version 7.1 \
  -o json
```

## Response Format

```json
{
  "daysOff": [
    {
      "start": "2024-12-25T00:00:00Z",
      "end": "2024-12-26T00:00:00Z"
    },
    {
      "start": "2024-01-01T00:00:00Z",
      "end": "2024-01-01T00:00:00Z"
    }
  ],
  "url": "..."
}
```

## Set Team Days Off

### Step 1: Create days off JSON

```json
{
  "daysOff": [
    {
      "start": "2024-12-25T00:00:00Z",
      "end": "2024-12-26T00:00:00Z"
    },
    {
      "start": "2024-01-01T00:00:00Z",
      "end": "2024-01-01T00:00:00Z"
    }
  ]
}
```

### Step 2: Update days off

```
az devops invoke \
  --area work \
  --resource teamdaysoff \
  --route-parameters project={PROJECT} team={TEAM} iterationId={ITERATION_ID} \
  --http-method PATCH \
  --in-file daysoff.json \
  --api-version 7.1 \
  -o json
```

## Notes

- Dates should be in ISO 8601 format
- Days off affect capacity calculations
- For single day, start and end are the same date
- These are team-wide; individual days off are in capacity settings

## Clear All Days Off

```json
{
  "daysOff": []
}
```

Then PATCH to clear.
