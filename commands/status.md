---
description: Check Azure DevOps CLI configuration and test connectivity
allowed-tools: Bash(az --version:*), Bash(az extension:*), Bash(az devops:*), Bash(az boards:*)
---

# Azure DevOps Status Check

Check the current Azure DevOps CLI configuration and verify connectivity.

## Step 1: Check Azure CLI Version

Verify Azure CLI is installed:

```bash
az --version
```

Look for the version number. If this fails, Azure CLI is not installed or not in PATH.

## Step 2: Check DevOps Extension

Verify the azure-devops extension is installed:

```bash
az extension list --query "[?name=='azure-devops'].{name:name, version:version}" -o table
```

If empty, the extension needs to be installed with `az extension add --name azure-devops`.

## Step 3: Show Current Configuration

Display the current default organization and project:

```bash
az devops configure --list
```

Expected output shows:
- **organization**: Your Azure DevOps organization URL (e.g., `https://dev.azure.com/yourorg`)
- **project**: Your default project name

If either is blank, run `/ado:setup` to configure.

## Step 4: Test Connectivity

Run a simple query to verify authentication and access:

```bash
az boards query --wiql "SELECT [System.Id], [System.Title] FROM workitems WHERE [System.TeamProject] = @project" -o table --top 1
```

If this succeeds, the configuration is working.

## Interpreting Results

### All Green

If all checks pass:
- Azure CLI is installed
- DevOps extension is present
- Organization and project are configured
- Connectivity and authentication are working

Report: "Azure DevOps CLI is configured and working correctly."

### Common Issues

**Azure CLI not found:**
- Error: `'az' is not recognized` or `az: command not found`
- Fix: Install Azure CLI (see README for instructions)

**Extension not installed:**
- Extension list is empty
- Fix: Run `az extension add --name azure-devops`

**No defaults configured:**
- Organization or project is blank
- Fix: Run `/ado:setup` or `az devops configure --defaults organization=URL project=NAME`

**Authentication error:**
- Error: `Please run 'az login' to setup account`
- Fix: Run `az login` or `az devops login`

**Project not found:**
- Error: `TF401019: The project does not exist`
- Fix: Check project name is correct (case-sensitive)

**Organization not found:**
- Error: `TF400813: Resource not available`
- Fix: Verify organization URL format is `https://dev.azure.com/orgname` (not `orgname.visualstudio.com`)

## Output Format

Present results as a status report:

```
Azure DevOps Status
===================

Azure CLI Version: 2.x.x
DevOps Extension:  Installed (v1.x.x)

Configuration:
  Organization: https://dev.azure.com/yourorg
  Project:      YourProject

Connectivity:    OK (query returned successfully)

Status: Ready to use
```

Or if there are issues:

```
Azure DevOps Status
===================

Azure CLI Version: 2.x.x
DevOps Extension:  Installed (v1.x.x)

Configuration:
  Organization: https://dev.azure.com/yourorg
  Project:      (not configured)

Connectivity:    FAILED - No default project

Status: Run /ado:setup to configure defaults
```
