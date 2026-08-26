---
description: View team days off for a sprint
allowed-tools: Bash(az devops invoke:*), Bash(echo:*)
---

# Team Days Off

View team-wide days off (holidays, team events) for an iteration.

Updating team days off via `az devops invoke --http-method PATCH` is not on the verified path (`az devops invoke` PATCH can KeyError on some resources). This command is GET/view only.

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

## Notes

- Dates should be in ISO 8601 format
- Days off affect capacity calculations
- For single day, start and end are the same date
- These are team-wide; individual days off are in capacity settings
