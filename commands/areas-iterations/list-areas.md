---
description: List and manage project area paths
allowed-tools: Bash(az boards area:*)
---

# Area Paths

Manage project area paths for organizing work items.

## List All Project Areas

```
az boards area project list -o table
```

With full details:
```
az boards area project list --depth 5 -o json
```

## Show Specific Area

```
az boards area project show --path "{AREA_PATH}"
```

Example:
```
az boards area project show --path "MyProject\\Team A\\Backend"
```

## Create Area

```
az boards area project create --name "{NAME}" --path "{PARENT_PATH}"
```

Examples:
```
# Create top-level area
az boards area project create --name "Team A"

# Create nested area
az boards area project create --name "Backend" --path "MyProject\\Team A"
```

## Update/Move Area

```
az boards area project update --path "{CURRENT_PATH}" --name "{NEW_NAME}"
```

Move to new parent:
```
az boards area project update --path "{CURRENT_PATH}" --child-id {AREA_ID}
```

## Delete Area

```
az boards area project delete --path "{AREA_PATH}" -y
```

**Warning**: Deleting an area affects all work items in that area. Work items will need to be moved first or will move to parent.

## List Team Areas

See which areas are assigned to a team:
```
az boards area team list --team {TEAM} -o table
```

## Add Area to Team

```
az boards area team add --team {TEAM} --path "{AREA_PATH}"
```

## Remove Area from Team

```
az boards area team remove --team {TEAM} --path "{AREA_PATH}"
```

## Path Format Notes

- Paths use backslash: `Project\\Area\\SubArea`
- Project name is root of path
- Escape backslashes in shell: `"Project\\\\Area"` or use single quotes
