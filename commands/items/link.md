---
description: Link work items together (parent/child, related, etc.)
allowed-tools: Bash(az boards work-item relation:*)
---

# Link Work Items

Create relationships between work items.

## List Available Link Types

```
az boards work-item relation list-type -o table
```

Common link types:
- `System.LinkTypes.Hierarchy-Forward` - Parent (this item is parent of target)
- `System.LinkTypes.Hierarchy-Reverse` - Child (this item is child of target)
- `System.LinkTypes.Related` - Related
- `System.LinkTypes.Dependency-Forward` - Successor
- `System.LinkTypes.Dependency-Reverse` - Predecessor
- `System.LinkTypes.Duplicate-Forward` - Duplicate
- `Microsoft.VSTS.Common.Affects-Forward` - Affects (for bugs)

## Add a Link

```
az boards work-item relation add \
  --id {SOURCE_ID} \
  --relation-type {LINK_TYPE} \
  --target-id {TARGET_ID} \
  -o json
```

### Examples

Make item 123 a child of item 100:
```
az boards work-item relation add --id 123 --relation-type System.LinkTypes.Hierarchy-Reverse --target-id 100
```

Link two items as related:
```
az boards work-item relation add --id 123 --relation-type System.LinkTypes.Related --target-id 456
```

Set predecessor (123 depends on 456):
```
az boards work-item relation add --id 123 --relation-type System.LinkTypes.Dependency-Reverse --target-id 456
```

## View Links on a Work Item

```
az boards work-item show --id {ID} --expand relations -o json
```

Look at the `relations` array in the response.

## Remove a Link

```
az boards work-item relation remove \
  --id {SOURCE_ID} \
  --relation-type {LINK_TYPE} \
  --target-id {TARGET_ID} \
  -o json
```

## Show Work Item with Friendly Link Names

```
az boards work-item relation show --id {ID} -o json
```

This shows relations with resolved names instead of just URLs.
