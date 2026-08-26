---
description: Link work items together (parent/child, related, etc.)
allowed-tools: Bash(az boards work-item relation:*)
---

# Link Work Items

Create relationships between work items.

`--relation-type` takes friendly names in quotes (`"Parent"`, `"Child"`, `"Related"`), not `System.LinkTypes.*` reference names.

## List Available Link Types

```
az boards work-item relation list-type -o table
```

Use the **Name** column from that command. Common values:

- `"Parent"` — this item becomes a child of the target (this item gains a parent)
- `"Child"` — this item becomes the parent of the target (this item gains a child)
- `"Related"` — related
- `"Predecessor"` — this item depends on the target
- `"Successor"` — the target depends on this item
- `"Duplicate"` — duplicate

## Add a Link

```
az boards work-item relation add \
  --id {SOURCE_ID} \
  --relation-type "Parent" \
  --target-id {TARGET_ID} \
  -o json
```

### Examples

Make item 123 a child of item 100 (123 gains a parent):
```
az boards work-item relation add --id 123 --relation-type "Parent" --target-id 100
```

Make item 123 a parent of item 456 (123 gains a child):
```
az boards work-item relation add --id 123 --relation-type "Child" --target-id 456
```

Link two items as related:
```
az boards work-item relation add --id 123 --relation-type "Related" --target-id 456
```

Set predecessor (123 depends on 456):
```
az boards work-item relation add --id 123 --relation-type "Predecessor" --target-id 456
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
  --relation-type "Parent" \
  --target-id {TARGET_ID} \
  -o json
```

## Show Work Item with Friendly Link Names

```
az boards work-item relation show --id {ID} -o json
```

This shows relations with resolved names instead of just URLs.
