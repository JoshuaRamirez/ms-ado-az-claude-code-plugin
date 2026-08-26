---
description: View and add comments on work items
allowed-tools: Bash(az boards work-item update:*), Bash(az devops invoke:*)
---

# Work Item Comments

View and add discussion comments on work items.

Add a comment with `az boards work-item update --discussion`. Discussion text is stored as `System.History` on the work-item revision that added it.

## Add a Comment

```bash
az boards work-item update --id {ID} --discussion "This is a comment" -o json
```

You can also add a comment while updating other fields:

```bash
az boards work-item update --id {ID} \
  --state "Resolved" \
  --discussion "Fixed in commit abc123. Please verify." \
  -o json
```

## List Comments

Discussion comments live on work item revisions as `System.History`. Use the same revisions invoke as `/ado:items:history`:

```bash
az devops invoke \
  --area wit \
  --resource revisions \
  --route-parameters project={PROJECT} id={ID} \
  --api-version 7.1 \
  -o json
```

**Note**: The `project` parameter should match your configured project. You can check it with `az devops configure --list`.

Each revision that added a discussion comment includes:
- `rev` - Revision number
- `fields.System.ChangedDate` - When the comment was added
- `fields.System.ChangedBy` - Who added it
- `fields.System.History` - The discussion text

Revisions that did not add a comment typically omit `System.History`. Filter to revisions that have that field.

Optional JMESPath to project comment fields:

```bash
az devops invoke \
  --area wit \
  --resource revisions \
  --route-parameters project={PROJECT} id={ID} \
  --api-version 7.1 \
  --query "[?fields.\"System.History\"].{rev:rev, date:fields.\"System.ChangedDate\", by:fields.\"System.ChangedBy\", comment:fields.\"System.History\"}" \
  -o json
```

## Edit or Delete a Comment

Editing or deleting an existing comment is not available on the verified CLI path. Add a new discussion comment instead.

## Notes

- `--discussion` is the verified way to add a comment
- Listing uses work-item revisions (`--resource revisions`), not a separate comments resource
- Discussion text is `System.History` on the revision that added it
