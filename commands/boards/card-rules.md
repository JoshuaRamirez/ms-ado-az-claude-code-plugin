---
description: View card styling rules on boards
allowed-tools: Bash(az devops invoke:*)
---

# Board Card Rules

View how cards appear on the board based on conditions.

Updating card rules via `az devops invoke --http-method PATCH` is not on the verified path (`az devops invoke` PATCH can KeyError on some resources). This command is GET/view only.

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

## Common Colors

Colors that appear in GET responses:

- Red: `#EC001D`
- Yellow: `#FBBC3D`
- Green: `#339933`
- Blue: `#0078D4`
- Purple: `#773B93`
- Orange: `#F58B1F`
