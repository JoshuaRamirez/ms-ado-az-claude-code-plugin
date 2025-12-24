# Azure DevOps Work Items Plugin

A Claude Code plugin for managing Azure DevOps work items, boards, backlogs, and sprints using the Azure CLI (`az devops`).

## Why This Plugin?

This plugin uses the official Azure CLI with the `azure-devops` extension instead of the Microsoft MCP server, which has known stability issues. The Azure CLI provides a reliable, well-documented interface to Azure DevOps APIs.

## Prerequisites

### 1. Azure CLI

Install the Azure CLI:
- **Windows**: `winget install Microsoft.AzureCLI` or [download installer](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows)
- **macOS**: `brew install azure-cli`
- **Linux**: See [installation guide](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux)

### 2. Azure DevOps Extension

Install the Azure DevOps extension for Azure CLI:

```bash
az extension add --name azure-devops
```

### 3. Authentication

Log in to Azure:

```bash
az login
```

Or use a Personal Access Token (PAT):

```bash
az devops login
```

### 4. Configure Defaults

Set your default organization and project:

```bash
az devops configure --defaults organization=https://dev.azure.com/YOUR_ORG project=YOUR_PROJECT
```

Verify configuration:

```bash
az devops configure --list
```

## Installation

### From Claude Plugin Marketplace

```bash
claude plugin marketplace add ms-ado-az
```

### From Local Source

```bash
claude plugin install /path/to/ado
```

## Available Commands

### Setup & Status

| Command | Description |
|---------|-------------|
| `/ado:setup` | Configure Azure DevOps CLI authentication and defaults |
| `/ado:status` | Check current configuration and test connectivity |

### Work Items

| Command | Description |
|---------|-------------|
| `/ado:items:show` | Show details of a work item by ID |
| `/ado:items:create` | Create a new work item (bug, task, user story, etc.) |
| `/ado:items:update` | Update an existing work item |
| `/ado:items:query` | Query work items using WIQL |
| `/ado:items:search` | Full-text search across work items |
| `/ado:items:link` | Link work items together (parent/child, related, etc.) |
| `/ado:items:comments` | View and add comments to work items |

### Boards

| Command | Description |
|---------|-------------|
| `/ado:boards:list` | List boards for a team |
| `/ado:boards:columns` | Get board column configuration |
| `/ado:boards:card-rules` | View board card styling rules |

### Backlogs

| Command | Description |
|---------|-------------|
| `/ado:backlogs:list` | List backlogs for a team |
| `/ado:backlogs:items` | Get items in a backlog |

### Sprints

| Command | Description |
|---------|-------------|
| `/ado:sprints:list` | List team iterations/sprints |
| `/ado:sprints:capacity` | View team capacity for a sprint |
| `/ado:sprints:days-off` | View team days off for a sprint |

### Areas & Iterations

| Command | Description |
|---------|-------------|
| `/ado:areas-iterations:list-areas` | List project area paths |
| `/ado:areas-iterations:list-iterations` | List project iteration paths |
| `/ado:areas-iterations:assign-team` | Assign areas or iterations to a team |

## Available Skills

Skills provide reference documentation and guidance for complex operations:

| Skill | Description |
|-------|-------------|
| `wiql-queries` | Build and execute WIQL (Work Item Query Language) queries |
| `wit-invoke` | Call Azure DevOps REST APIs via `az devops invoke` |
| `hierarchy-tree` | Construct ASCII tree visualizations from parent/child work item data |
| `work-item-fields` | Reference for work item fields, link types, and states |

## Agent

The plugin includes an AI agent for interactive work item management:

### `ado-work-items` Agent

An Azure DevOps work item assistant that can:
- Query and search work items
- Create and update work items
- Manage boards and backlogs
- Handle sprints and capacity
- Work with areas and iterations

Start the agent with:

```
/agent ado-work-items
```

## Examples

### Query My Open Tasks

```
/ado:items:query
```

Then ask for: "Show me all tasks assigned to me that are not done"

### Create a Bug

```
/ado:items:create
```

Then provide: Type "Bug", Title "Login button not responding", Priority 1

### View Sprint Capacity

```
/ado:sprints:capacity
```

Then specify the team and sprint iteration.

## Troubleshooting

### Check Status

Run `/ado:status` to verify:
- Azure CLI is installed
- Azure DevOps extension is present
- Default organization and project are configured
- Connectivity to Azure DevOps is working

### Common Issues

1. **"az: command not found"** - Azure CLI is not installed or not in PATH
2. **"The following extension is not installed: azure-devops"** - Run `az extension add --name azure-devops`
3. **"Please run 'az login' to setup account"** - Run `az login` to authenticate
4. **"TF401019: The project does not exist"** - Check your project name is correct (case-sensitive)
5. **"TF400813: Resource not available"** - Verify organization URL format is `https://dev.azure.com/orgname`

### Re-authenticate

```bash
az account clear
az login
az devops configure --defaults organization=https://dev.azure.com/YOUR_ORG project=YOUR_PROJECT
```

## License

MIT License - see [LICENSE](LICENSE) for details.

## Author

Joshua Ramirez
