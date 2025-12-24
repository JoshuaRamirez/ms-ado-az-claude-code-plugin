---
description: Create a new work item (bug, task, user story, etc.)
allowed-tools: Bash(az boards work-item create:*)
---

# Create Work Item

Create a new work item of any type.

## Required Information

Ask user for:
1. **Type**: Bug, Task, User Story, Feature, Epic, Issue, etc.
2. **Title**: Required

## Basic Creation

```
az boards work-item create \
  --type "{Type}" \
  --title "{Title}" \
  -o json
```

## With Additional Fields

```
az boards work-item create \
  --type "{Type}" \
  --title "{Title}" \
  --assigned-to "{Email or Name}" \
  --area "{Area Path}" \
  --iteration "{Iteration Path}" \
  --description "{Description}" \
  -o json
```

## Examples by Work Item Type

### Create a Bug

```bash
az boards work-item create \
  --type "Bug" \
  --title "Login button not responding on mobile" \
  --assigned-to "developer@example.com" \
  --fields "Microsoft.VSTS.Common.Priority=1" \
           "Microsoft.VSTS.Common.Severity=2 - High" \
           "Microsoft.VSTS.TCM.ReproSteps=<ol><li>Open mobile browser</li><li>Navigate to login</li><li>Tap login button</li><li>Nothing happens</li></ol>" \
           "System.Tags=mobile,urgent,login"
```

### Create a User Story

```bash
az boards work-item create \
  --type "User Story" \
  --title "As a user, I want to reset my password via email" \
  --fields "Microsoft.VSTS.Scheduling.StoryPoints=5" \
           "Microsoft.VSTS.Common.AcceptanceCriteria=<ul><li>User receives reset email within 1 minute</li><li>Reset link expires after 24 hours</li><li>Password requirements displayed</li></ul>" \
           "Microsoft.VSTS.Common.Priority=2" \
           "System.Tags=authentication,user-management"
```

### Create a Task

```bash
az boards work-item create \
  --type "Task" \
  --title "Implement password validation logic" \
  --assigned-to "developer@example.com" \
  --iteration "MyProject\\Sprint 5" \
  --fields "Microsoft.VSTS.Scheduling.RemainingWork=4" \
           "Microsoft.VSTS.Scheduling.OriginalEstimate=4" \
           "Microsoft.VSTS.Common.Activity=Development" \
           "Microsoft.VSTS.Common.Priority=2"
```

### Create a Feature

```bash
az boards work-item create \
  --type "Feature" \
  --title "User Authentication System" \
  --fields "Microsoft.VSTS.Scheduling.TargetDate=2024-06-30" \
           "Microsoft.VSTS.Common.BusinessValue=100" \
           "Microsoft.VSTS.Common.TimeCriticality=2" \
           "System.Tags=security,q2-release"
```

### Create an Epic

```bash
az boards work-item create \
  --type "Epic" \
  --title "Platform Security Overhaul" \
  --description "<p>Complete security review and implementation of modern authentication standards including:</p><ul><li>OAuth 2.0</li><li>MFA</li><li>SSO integration</li></ul>" \
  --fields "Microsoft.VSTS.Common.Priority=1" \
           "Microsoft.VSTS.Scheduling.StartDate=2024-01-01" \
           "Microsoft.VSTS.Scheduling.TargetDate=2024-12-31"
```

### Create an Issue (Agile process)

```bash
az boards work-item create \
  --type "Issue" \
  --title "Investigate production memory leak" \
  --assigned-to "oncall@example.com" \
  --fields "Microsoft.VSTS.Common.Priority=1" \
           "System.Tags=production,investigation,memory"
```

## Field Reference by Category

### Priority and Severity

| Field Path | Values | Applicable To |
|------------|--------|---------------|
| Microsoft.VSTS.Common.Priority | 1 (highest) to 4 (lowest) | All types |
| Microsoft.VSTS.Common.Severity | 1 - Critical, 2 - High, 3 - Medium, 4 - Low | Bugs |

### Scheduling and Effort

| Field Path | Description | Applicable To |
|------------|-------------|---------------|
| Microsoft.VSTS.Scheduling.StoryPoints | Relative effort estimate | User Stories |
| Microsoft.VSTS.Scheduling.Effort | Effort value | Features, Epics |
| Microsoft.VSTS.Scheduling.OriginalEstimate | Hours originally estimated | Tasks |
| Microsoft.VSTS.Scheduling.RemainingWork | Hours remaining | Tasks |
| Microsoft.VSTS.Scheduling.CompletedWork | Hours completed | Tasks |
| Microsoft.VSTS.Scheduling.StartDate | Start date (YYYY-MM-DD) | Features, Epics |
| Microsoft.VSTS.Scheduling.TargetDate | Target completion date | Features, Epics |

### Activity and Classification

| Field Path | Values/Description | Applicable To |
|------------|-------------------|---------------|
| Microsoft.VSTS.Common.Activity | Development, Testing, Documentation, Design, Requirements, Deployment | Tasks |
| Microsoft.VSTS.Common.BusinessValue | Numeric value (higher = more valuable) | Features |
| Microsoft.VSTS.Common.TimeCriticality | 1 (most critical) to 4 | Features |
| Microsoft.VSTS.Common.Risk | 1 - High, 2 - Medium, 3 - Low | User Stories, Features |
| Microsoft.VSTS.Common.ValueArea | Business, Architectural | User Stories, Features |

### Bug-Specific Fields

| Field Path | Description |
|------------|-------------|
| Microsoft.VSTS.TCM.ReproSteps | HTML-formatted reproduction steps |
| Microsoft.VSTS.TCM.SystemInfo | System/environment information |
| Microsoft.VSTS.Common.FoundIn | Build or version where bug was found |
| Microsoft.VSTS.Build.IntegrationBuild | Build where bug was fixed |

### User Story-Specific Fields

| Field Path | Description |
|------------|-------------|
| Microsoft.VSTS.Common.AcceptanceCriteria | HTML-formatted acceptance criteria |

### Common Fields (All Types)

| Field Path | Description |
|------------|-------------|
| System.AssignedTo | Email or display name |
| System.AreaPath | Area path (e.g., Project\\Team\\Component) |
| System.IterationPath | Sprint/iteration path |
| System.Tags | Comma-separated tags |
| System.Description | HTML description |

## Working with HTML Content

For fields that accept HTML (Description, ReproSteps, AcceptanceCriteria), use HTML tags:

```bash
--description "<p>First paragraph.</p><p>Second paragraph with <b>bold</b> text.</p>"
```

Common HTML elements:
- `<p>...</p>` - Paragraphs
- `<ul><li>...</li></ul>` - Unordered lists
- `<ol><li>...</li></ol>` - Numbered lists
- `<b>...</b>` - Bold text
- `<br>` - Line break

## After Creation

Display the new work item ID and URL to the user. The response includes:
- `id` - The new work item ID
- `url` - API URL to the work item
- `_links.html.href` - Web URL for viewing in browser
