---
description: Configure Azure DevOps CLI authentication and defaults
allowed-tools: Bash(az login:*), Bash(az devops:*), Bash(az extension:*), Bash(az account:*)
---

# Azure DevOps Setup

Guide the user through Azure DevOps CLI configuration.

## Step 1: Check Azure CLI

Verify Azure CLI is installed and check if devops extension is present:

```
az --version
az extension list --query "[?name=='azure-devops']"
```

If extension not installed:
```
az extension add --name azure-devops
```

## Step 2: Authentication

Check current login status:
```
az account show
```

If not logged in, guide user through:
```
az login
```

For PAT-based auth (alternative):
```
az devops login
```

## Step 3: Configure Defaults

Ask user for their organization URL and default project:

```
az devops configure --defaults organization=https://dev.azure.com/{ORG} project={PROJECT}
```

Verify configuration:
```
az devops configure --list
```

## Step 4: Test Connection

Run a simple query to verify everything works:
```
az devops project show --project {PROJECT}
```

## Troubleshooting

If commands fail:
1. Verify org URL format: `https://dev.azure.com/{orgname}` (not `{orgname}.visualstudio.com`)
2. Check project name is correct (case-sensitive)
3. Verify user has access to the project
4. Try `az account clear` and `az login` again
