# ADO Plugin - Verified Working Commands

Tested on: 2024-12-22
Organization: AspenESS
Project: ResearchAndDevelopment

## Work Items - CLI Commands

### Show Work Item
```bash
# Basic show
az boards work-item show --id 1839 -o table

# With specific fields
az boards work-item show --id 1839 --fields "System.Id,System.Title,System.State" -o json

# With relations expanded
az boards work-item show --id 1839 --expand relations -o json
```

### Query Work Items (WIQL)
```bash
# Basic query - explicit project name
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State] FROM workitems WHERE [System.TeamProject] = 'ResearchAndDevelopment'" -o table

# Query with ORDER BY
az boards query --wiql "SELECT [System.Id], [System.Title], [System.State] FROM workitems WHERE [System.TeamProject] = 'ResearchAndDevelopment' ORDER BY [System.ChangedDate] DESC" -o table

# Query by state
az boards query --wiql "SELECT [System.Id], [System.Title] FROM workitems WHERE [System.State] = 'Done' AND [System.TeamProject] = 'ResearchAndDevelopment'" -o table

# Query by work item type
az boards query --wiql "SELECT [System.Id], [System.Title] FROM workitems WHERE [System.WorkItemType] = 'Feature' AND [System.TeamProject] = 'ResearchAndDevelopment'" -o table

# Query by assignee
az boards query --wiql "SELECT [System.Id], [System.Title] FROM workitems WHERE [System.AssignedTo] = 'JRamirez@aspeness.com' AND [System.TeamProject] = 'ResearchAndDevelopment'" -o table
```

**WIQL LIMITATIONS:**
- NO `TOP N` clause - WIQL doesn't support SQL Server TOP syntax
- Use `@project` macro with caution - sometimes requires explicit project name
- Limit results with shell: `| head -N`

### Create Work Item
```bash
az boards work-item create --type "Task" --title "My new task" --description "Description here" -o json

# With assignment and iteration
az boards work-item create --type "Bug" --title "Bug title" --assigned-to "user@email.com" --iteration "Project\Sprint 1" -o json

# With custom fields
az boards work-item create --type "Feature" --title "Feature title" --fields "Microsoft.VSTS.Common.Priority=1" -o json
```

### Update Work Item
```bash
az boards work-item update --id 1839 --state "In Progress" -o json

# Update multiple fields
az boards work-item update --id 1839 --title "New title" --assigned-to "user@email.com" -o json

# Add discussion comment
az boards work-item update --id 1839 --discussion "This is a comment" -o json
```

### Work Item Relations
```bash
# List relation types
az boards work-item relation list-type -o table

# Show work item with relations
az boards work-item relation show --id 1839 -o json

# Add child relation (1839 becomes parent of 1840)
az boards work-item relation add --id 1839 --relation-type "Child" --target-id 1840 -o json

# Add parent relation (1839 becomes child of 1840)
az boards work-item relation add --id 1839 --relation-type "Parent" --target-id 1840 -o json
```

## Areas and Iterations - CLI Commands

### Areas
```bash
# List team areas
az boards area team list --team "ResearchAndDevelopment Team" -o table

# List project areas
az boards area project list -o table
```

### Iterations
```bash
# List team iterations
az boards iteration team list --team "ResearchAndDevelopment Team" -o table

# List project iterations
az boards iteration project list -o table
```

## REST API via az devops invoke

### Search Work Items
```bash
# Create search payload file
echo '{"searchText": "pipeline", "$top": 10}' > /tmp/search.json

# Execute search
az devops invoke --area search --resource workitemsearchresults --http-method POST --in-file /tmp/search.json --api-version 7.0 -o json

# With JMESPath query for specific fields
az devops invoke --area search --resource workitemsearchresults --http-method POST --in-file /tmp/search.json --api-version 7.0 --query "results[].{id:fields.\"system.id\", title:fields.\"system.title\", state:fields.\"system.state\"}" -o table
```

**SEARCH LIMITATIONS:**
- Requires POST with JSON body
- `--in-file -` does NOT work - must use actual file path
- Use `/tmp/search.json` or similar temp file

### List Boards
```bash
az devops invoke --area work --resource boards --route-parameters project=ResearchAndDevelopment team="ResearchAndDevelopment Team" --api-version 7.0 -o json
```

### Get Board Columns
```bash
az devops invoke --area work --resource columns --route-parameters project=ResearchAndDevelopment team="ResearchAndDevelopment Team" board=Features --api-version 7.0 -o json
```

### Get Backlogs
```bash
az devops invoke --area work --resource backlogs --route-parameters project=ResearchAndDevelopment team="ResearchAndDevelopment Team" --api-version 7.0 -o json
```

### Get Team Capacity
```bash
az devops invoke --area work --resource capacities --route-parameters project=ResearchAndDevelopment team="ResearchAndDevelopment Team" iterationId="{iteration-id}" --api-version 7.0 -o json
```

## Common Field Reference Names

| Display Name | Reference Name |
|--------------|----------------|
| ID | System.Id |
| Title | System.Title |
| State | System.State |
| Assigned To | System.AssignedTo |
| Work Item Type | System.WorkItemType |
| Area Path | System.AreaPath |
| Iteration Path | System.IterationPath |
| Created Date | System.CreatedDate |
| Changed Date | System.ChangedDate |
| Priority | Microsoft.VSTS.Common.Priority |
| Tags | System.Tags |
| Description | System.Description |

## Common Link Types

| Display Name | Reference Name |
|--------------|----------------|
| Parent | System.LinkTypes.Hierarchy-Reverse |
| Child | System.LinkTypes.Hierarchy-Forward |
| Related | System.LinkTypes.Related |
| Predecessor | System.LinkTypes.Dependency-Reverse |
| Successor | System.LinkTypes.Dependency-Forward |
| Duplicate | System.LinkTypes.Duplicate-Forward |
| Duplicate Of | System.LinkTypes.Duplicate-Reverse |

## Known Issues and Workarounds

1. **WIQL TOP not supported** - Pipe to `head -N` instead
2. **@project macro unreliable** - Use explicit project name in WHERE clause
3. **--in-file requires file path** - Cannot use stdin (`-`)
4. **Comments API** - Not at `/wit/workItems/{id}/comments` - use discussion field on update instead
5. **Route parameters** - Some invoke calls fail with KeyError if route template doesn't match exactly

## Known CLI Gotchas

A consolidated reference of CLI behavior that may be unexpected:

| Gotcha | Expected Behavior | Actual Behavior | Workaround |
|--------|-------------------|-----------------|------------|
| No `--top` flag on `az boards query` | Limit results with a flag | Flag doesn't exist | Use WIQL or pipe to `head -N` |
| No `TOP` clause in WIQL | Use SQL-style `SELECT TOP N` | Syntax error | Pipe results to `head -N` |
| `--in-file` doesn't accept stdin | Use `-` for stdin like other tools | Fails with file not found | Must use actual file path (e.g., `/tmp/payload.json`) |
| Comments API returns 404 | `az devops invoke` to `/wit/workItems/{id}/comments` | 404 Not Found | Use `--discussion` flag on `az boards work-item update` instead |
| Relation types use friendly names | Use reference name like `System.LinkTypes.Hierarchy-Reverse` | May fail or behave unexpectedly | Use friendly names: `"Parent"`, `"Child"`, `"Related"` |
| `@project` macro unreliable | WIQL uses `@project` to reference current project | Sometimes fails to resolve | Use explicit project name in WIQL WHERE clause |
| `--project` on work-item show | Expect `--project` flag support | `error: unrecognized arguments: --project` | Omit `--project`; defaults are used automatically |
| `--fields` with `--expand` | Use both flags together | Cannot be used together (API restriction) | Use `--expand` alone, then filter JSON output |
| Setting System.BoardColumn directly | Set board column via field update | Column is derived from state; not directly settable | Change `System.State` to a state mapped to target column |
| `az boards board column list` | CLI command exists | Command doesn't exist | Use `az devops invoke --area work --resource columns` |
| `az devops invoke` with PATCH | Expect PATCH support | KeyError issues on some resources | Use update CLI commands or POST with appropriate resources |
| WIQL `LIKE` operator | SQL-style LIKE support | Not supported | Use `CONTAINS` instead |
| WIQL `ORDER BY [System.Parent]` | Sort by parent ID | Not supported | Filter by parent with IN clause, sort client-side |

## Board Column Behavior

Board columns work differently than expected:

1. **Board columns are mapped to states** - Each board has a configuration mapping columns to states
2. **Cannot directly set System.BoardColumn** - It's a derived field controlled by state-to-column mappings
3. **Different boards have different column configurations** - Features board vs Stories board may differ
4. **Kanban column fields have team-specific GUIDs** - Format: `WEF_{GUID}_Kanban.Column`

### Discovering Board Column Mappings

```bash
# List boards for a team
az devops invoke --area work --resource boards \
  --route-parameters project=ProjectName team="Team Name" \
  --api-version 7.1 -o json

# Get column configuration for a specific board
az devops invoke --area work --resource columns \
  --route-parameters project=ProjectName team="Team Name" board=Features \
  --api-version 7.1 -o json
```

## Efficient Bulk Operations

### Performance Insight
Direct bash for loops are approximately **10x faster** than using sub-agents for bulk operations.

### Recommended Patterns

```bash
# Fast bulk state update (use -o none to suppress output for speed)
for id in 1858 1859 1860; do
  az boards work-item update --id $id --state "Done" -o none && echo "Updated $id"
done

# Query with table output for quick counts
az boards query --wiql "SELECT [System.Id] FROM WorkItems WHERE ..." --output table | wc -l

# Find field names on a work item
az boards work-item show --id 1847 -o json | grep -i "column\|board\|kanban"
```

## Field Discovery Techniques

```bash
# Find all fields containing a keyword
az boards work-item show --id {ID} -o json | grep -i "keyword"

# Get all field names using --query (JMESPath)
az boards work-item show --id {ID} --query "keys(fields)" -o json

# Get specific field value
az boards work-item show --id {ID} --query "fields.\"System.Title\"" -o tsv

# Discover Kanban column field names (team-specific GUIDs)
az boards work-item show --id {ID} -o json | grep -i "WEF_"
```
