---
description: View and add comments on work items
allowed-tools: Bash(az devops invoke:*), Bash(echo:*), Write
---

# Work Item Comments

View and add discussion comments on work items.

**Note**: Uses `az devops invoke` since CLI doesn't support comments.

## List Comments

```
az devops invoke \
  --area wit \
  --resource comments \
  --route-parameters project={PROJECT} workItemId={ID} \
  --api-version 7.1-preview.4 \
  -o json
```

Response contains `comments` array with:
- `id` - Comment ID
- `text` - Comment content (may contain HTML)
- `createdBy` - Author info
- `createdDate` - Timestamp
- `modifiedDate` - Last edit timestamp

## Add a Comment

### Step 1: Create comment JSON

```json
{
  "text": "Your comment text here. Can include <b>HTML</b> formatting."
}
```

Save to temp file, e.g., `comment.json`

### Step 2: Post comment

```
az devops invoke \
  --area wit \
  --resource comments \
  --route-parameters project={PROJECT} workItemId={ID} \
  --http-method POST \
  --in-file comment.json \
  --api-version 7.1-preview.4 \
  -o json
```

## Update a Comment

### Step 1: Create update JSON

```json
{
  "text": "Updated comment text"
}
```

### Step 2: Patch comment

```
az devops invoke \
  --area wit \
  --resource comments \
  --route-parameters project={PROJECT} workItemId={ID} commentId={COMMENT_ID} \
  --http-method PATCH \
  --in-file comment_update.json \
  --api-version 7.1-preview.4 \
  -o json
```

## Delete a Comment

```
az devops invoke \
  --area wit \
  --resource comments \
  --route-parameters project={PROJECT} workItemId={ID} commentId={COMMENT_ID} \
  --http-method DELETE \
  --api-version 7.1-preview.4
```

## Formatting Tips

Comments support HTML:
- `<b>bold</b>`, `<i>italic</i>`
- `<a href="url">link</a>`
- `<ul><li>list items</li></ul>`
- `@mention` for user mentions
- `#123` for work item links

## Clean Up

Delete temp JSON files after operations complete.
