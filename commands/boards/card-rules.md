---
description: View and manage card styling rules on boards
allowed-tools: Bash(az devops invoke:*), Bash(echo:*), Write
---

# Board Card Rules

Configure how cards appear on the board based on conditions.

## Get Card Rules

```
az devops invoke \
  --area work \
  --resource cardrulesettings \
  --route-parameters project={PROJECT} team={TEAM} board={BOARD} \
  --api-version 7.1 \
  -o json
```

## Response Format

```json
{
  "rules": {
    "fill": [
      {
        "name": "High Priority",
        "isEnabled": true,
        "filter": "[Microsoft.VSTS.Common.Priority] = '1'",
        "settings": {
          "background-color": "#FBBC3D",
          "title-color": "#000000"
        }
      }
    ],
    "tagStyle": [
      {
        "name": "urgent",
        "isEnabled": true,
        "settings": {
          "background-color": "#EC001D",
          "color": "#FFFFFF"
        }
      }
    ]
  }
}
```

Rule types:
- `fill` - Card background/title color based on field conditions
- `tagStyle` - Tag appearance by tag name

## Update Card Rules

### Step 1: Create rules JSON

```json
{
  "rules": {
    "fill": [
      {
        "name": "Blocked",
        "isEnabled": true,
        "filter": "[System.Tags] CONTAINS 'blocked'",
        "settings": {
          "background-color": "#EC001D",
          "title-color": "#FFFFFF"
        }
      },
      {
        "name": "Priority 1",
        "isEnabled": true,
        "filter": "[Microsoft.VSTS.Common.Priority] = '1'",
        "settings": {
          "background-color": "#FBBC3D"
        }
      }
    ],
    "tagStyle": [
      {
        "name": "urgent",
        "isEnabled": true,
        "settings": {
          "background-color": "#EC001D",
          "color": "#FFFFFF"
        }
      },
      {
        "name": "tech-debt",
        "isEnabled": true,
        "settings": {
          "background-color": "#773B93",
          "color": "#FFFFFF"
        }
      }
    ]
  }
}
```

### Step 2: Apply rules

```
az devops invoke \
  --area work \
  --resource cardrulesettings \
  --route-parameters project={PROJECT} team={TEAM} board={BOARD} \
  --http-method PATCH \
  --in-file card_rules.json \
  --api-version 7.1 \
  -o json
```

## Common Colors

- Red: `#EC001D`
- Yellow: `#FBBC3D`
- Green: `#339933`
- Blue: `#0078D4`
- Purple: `#773B93`
- Orange: `#F58B1F`
