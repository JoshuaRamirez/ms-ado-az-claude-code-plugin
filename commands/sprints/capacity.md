---
description: View and manage team capacity for a sprint
allowed-tools: Bash(az devops invoke:*), Bash(echo:*), Write
---

# Sprint Capacity

Manage team member capacity for an iteration.

## Get Team Capacities

```
az devops invoke \
  --area work \
  --resource capacities \
  --route-parameters project={PROJECT} team={TEAM} iterationId={ITERATION_ID} \
  --api-version 7.1 \
  -o json
```

## Response Format

```json
{
  "value": [
    {
      "teamMember": {
        "id": "...",
        "displayName": "John Doe"
      },
      "activities": [
        {
          "name": "Development",
          "capacityPerDay": 6
        }
      ],
      "daysOff": [
        {"start": "2024-01-15", "end": "2024-01-16"}
      ]
    }
  ]
}
```

## Get Total Capacity

```
az devops invoke \
  --area work \
  --resource iterationcapacities \
  --route-parameters project={PROJECT} team={TEAM} iterationId={ITERATION_ID} \
  --api-version 7.1 \
  -o json
```

## Update Member Capacity

### Step 1: Create capacity JSON

```json
{
  "activities": [
    {
      "name": "Development",
      "capacityPerDay": 6
    }
  ],
  "daysOff": []
}
```

### Step 2: Update capacity

```
az devops invoke \
  --area work \
  --resource capacities \
  --route-parameters project={PROJECT} team={TEAM} iterationId={ITERATION_ID} teamMemberId={MEMBER_ID} \
  --http-method PATCH \
  --in-file capacity.json \
  --api-version 7.1 \
  -o json
```

## Activity Types

Common activities:
- `Development`
- `Testing`
- `Design`
- `Documentation`
- `Deployment`
- `Requirements`

These should match your project's activity settings.
