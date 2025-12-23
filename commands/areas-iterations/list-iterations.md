---
description: List and manage project iteration paths
allowed-tools: Bash(az boards iteration:*)
---

# Iteration Paths

Manage project iterations (sprints) structure.

## List All Project Iterations

```
az boards iteration project list -o table
```

With hierarchy:
```
az boards iteration project list --depth 3 -o json
```

## Show Specific Iteration

```
az boards iteration project show --path "{ITERATION_PATH}"
```

Example:
```
az boards iteration project show --path "MyProject\\Release 1\\Sprint 5"
```

## Create Iteration

```
az boards iteration project create --name "{NAME}" --path "{PARENT_PATH}"
```

Examples:
```
# Create release container
az boards iteration project create --name "Release 2"

# Create sprint under release
az boards iteration project create --name "Sprint 1" --path "MyProject\\Release 2"
```

With dates:
```
az boards iteration project create \
  --name "Sprint 1" \
  --path "MyProject\\Release 2" \
  --start-date "2024-01-15" \
  --finish-date "2024-01-28"
```

## Update Iteration

Change name:
```
az boards iteration project update --path "{PATH}" --name "{NEW_NAME}"
```

Update dates:
```
az boards iteration project update \
  --path "{PATH}" \
  --start-date "2024-01-15" \
  --finish-date "2024-01-28"
```

## Delete Iteration

```
az boards iteration project delete --path "{PATH}" -y
```

**Warning**: Work items in this iteration will need to be moved first.

## Iteration Date Formats

Use ISO format: `YYYY-MM-DD`
- `--start-date "2024-01-15"`
- `--finish-date "2024-01-28"`

## Common Structure

```
Project
├── Release 1
│   ├── Sprint 1
│   ├── Sprint 2
│   └── Sprint 3
├── Release 2
│   ├── Sprint 4
│   ├── Sprint 5
│   └── Sprint 6
└── Backlog (for unscheduled items)
```
