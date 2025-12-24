# Contributing to the Azure DevOps Plugin

Thank you for your interest in contributing to the Azure DevOps Work Items plugin for Claude Code. This guide explains how the plugin is structured and how to add new functionality.

## Plugin Structure

```
ado/
├── .claude-plugin/
│   ├── plugin.json          # Plugin metadata and configuration
│   └── marketplace.json     # Marketplace listing information
├── commands/                 # Slash commands (user-invocable)
│   ├── setup.md             # /ado:setup
│   ├── status.md            # /ado:status
│   ├── items/               # Work item commands
│   │   ├── show.md          # /ado:items:show
│   │   ├── create.md        # /ado:items:create
│   │   ├── update.md        # /ado:items:update
│   │   ├── query.md         # /ado:items:query
│   │   ├── search.md        # /ado:items:search
│   │   ├── link.md          # /ado:items:link
│   │   ├── comments.md      # /ado:items:comments
│   │   └── history.md       # /ado:items:history
│   ├── boards/              # Board commands
│   ├── backlogs/            # Backlog commands
│   ├── sprints/             # Sprint commands
│   └── areas-iterations/    # Area and iteration commands
├── skills/                   # Reference documentation for complex tasks
│   ├── wit-invoke/          # REST API via az devops invoke
│   ├── hierarchy-tree/      # Work item hierarchy visualization
│   ├── work-item-fields/    # Field reference
│   └── bulk-operations/     # Bulk update patterns
├── agents/                   # AI agents with broader capabilities
│   └── work-items.md        # Main work item management agent
├── tests/                    # Test documentation
│   └── verified-commands.md # Commands verified to work
├── README.md                 # User documentation
├── CONTRIBUTING.md           # This file
└── LICENSE                   # MIT License
```

## Key Concepts

### Commands vs Skills vs Agents

| Type | Purpose | User Invocation | Scope |
|------|---------|-----------------|-------|
| **Commands** | Single-purpose operations | `/ado:items:show` | Narrow, focused task |
| **Skills** | Reference documentation | Not directly invoked | Guidance for complex scenarios |
| **Agents** | Multi-step assistants | `/agent ado-work-items` | Broad, interactive capabilities |

### Commands

Commands are markdown files that define a specific action. When a user invokes a slash command, Claude reads the command file and follows its instructions.

**Key elements:**
- **Frontmatter**: Metadata including description and allowed tools
- **Instructions**: Step-by-step guidance for Claude
- **Examples**: Command syntax and usage patterns

### Skills

Skills provide reference documentation that Claude can consult when handling complex tasks. They are not directly user-invocable but inform how Claude handles certain scenarios.

### Agents

Agents are more capable assistants that can handle multi-step workflows. They have broader tool access and can combine multiple operations.

## Adding a New Command

### Step 1: Create the Command File

Create a new markdown file in the appropriate `commands/` subdirectory:

```bash
# Example: commands/items/my-command.md
```

### Step 2: Add Required Frontmatter

Every command file must have YAML frontmatter:

```yaml
---
description: Brief description of what the command does
allowed-tools: Bash(az boards work-item show:*), Bash(az devops invoke:*)
---
```

**Frontmatter fields:**
- `description` (required): One-line description shown in command listings
- `allowed-tools` (required): Tools the command can use (comma-separated)

### Step 3: Write the Command Content

Structure your command file with:

1. **Title**: What the command does
2. **Usage**: How to use it
3. **Required Information**: What the user must provide
4. **Command Examples**: Actual CLI commands with placeholders
5. **Output Format**: How to present results
6. **Notes/Troubleshooting**: Edge cases and common issues

### Step 4: Test the Command

1. Install the plugin locally: `claude plugin install /path/to/ado`
2. Invoke your command: `/ado:items:my-command`
3. Verify it works as expected

### Example Command File

```markdown
---
description: Show work item details by ID
allowed-tools: Bash(az boards work-item show:*)
---

# Show Work Item

Get work item details by ID.

## Usage

Ask user for the work item ID, then run:

\`\`\`bash
az boards work-item show --id {ID} -o json
\`\`\`

## Display Format

Present the key fields clearly:
- **ID**: System.Id
- **Title**: System.Title
- **State**: System.State

## Notes

- Use `--expand relations` to include linked items
```

## Adding a New Skill

### Step 1: Create the Skill Directory and File

```bash
mkdir skills/my-skill
# Create skills/my-skill/SKILL.md
```

### Step 2: Add Required Frontmatter

```yaml
---
name: my-skill
description: Detailed description of when this skill applies
---
```

### Step 3: Document the Skill

Include:
- When to use this approach
- Prerequisites and limitations
- Verified working examples
- Common pitfalls to avoid

## Adding a New Agent

### Step 1: Create the Agent File

Create a markdown file in the `agents/` directory:

```bash
# agents/my-agent.md
```

### Step 2: Add Required Frontmatter

```yaml
---
name: my-agent
description: What the agent helps with
tools: Bash
model: sonnet
---
```

### Step 3: Define Agent Capabilities

Document:
- What the agent can do
- Available operations and commands
- Workflow guidelines
- Error handling approaches

## Allowed Tools Reference

### Common Tool Patterns

```yaml
# Specific CLI command
allowed-tools: Bash(az boards work-item show:*)

# Multiple CLI commands
allowed-tools: Bash(az boards work-item show:*), Bash(az boards work-item update:*)

# REST API calls
allowed-tools: Bash(az devops invoke:*)

# File operations (for temp files)
allowed-tools: Bash(az devops invoke:*), Bash(echo:*), Write

# All bash commands (use sparingly)
allowed-tools: Bash
```

### Tool Pattern Syntax

- `Bash(command:*)` - Allow command with any arguments
- `Bash(command:subcommand:*)` - Allow specific subcommand
- Multiple tools separated by `, `

## Testing Locally

### Prerequisites

1. Install Azure CLI: `az --version`
2. Install DevOps extension: `az extension add --name azure-devops`
3. Authenticate: `az login`
4. Configure defaults: `az devops configure --defaults organization=... project=...`

### Installing the Plugin

```bash
# Install from local directory
claude plugin install C:\path\to\ado

# Or create symlink for development
claude plugin link C:\path\to\ado
```

### Testing Commands

```bash
# List all plugin commands
/help

# Test specific command
/ado:items:show

# Test with debug output
# (Commands output their actions to the chat)
```

### Verifying CLI Commands

Before documenting a command, verify it works:

```bash
# Test the actual CLI command
az boards work-item show --id 123 -o json

# For invoke commands, check the API first
az devops invoke -o json  # Lists all available APIs
```

## Submitting Changes

### Before Submitting

1. **Test your changes**: Verify commands work as expected
2. **Update documentation**: Add examples and notes
3. **Check frontmatter**: Ensure required fields are present
4. **Validate structure**: Run the validation workflow locally if possible

### Pull Request Process

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-new-command`
3. Make your changes
4. Commit with a clear message: `git commit -m "Add items:history command for revision tracking"`
5. Push to your fork: `git push origin feature/my-new-command`
6. Create a Pull Request

### Pull Request Guidelines

- **Title**: Clear, concise description of the change
- **Description**: Include:
  - What the change does
  - Why it's needed
  - How to test it
  - Any breaking changes
- **Testing**: Document how you tested the changes

## Code Style Guidelines

### Markdown

- Use ATX-style headers (`#`, `##`, `###`)
- Use fenced code blocks with language hints
- Keep lines under 120 characters where practical
- Use reference-style links for repeated URLs

### Command Examples

- Use placeholders like `{ID}`, `{PROJECT}`, `{USER}`
- Show both basic and advanced usage
- Include comments explaining non-obvious options
- Provide copy-pasteable examples

### YAML Frontmatter

- Keep frontmatter minimal
- Use lowercase field names
- Quote strings with special characters

## Common Issues

### Command Not Found

- Verify the file is in the correct `commands/` subdirectory
- Check the filename ends with `.md`
- Ensure frontmatter is valid YAML

### Tool Permission Denied

- Check the `allowed-tools` in frontmatter
- Verify the tool pattern matches the command being run

### CLI Command Fails

- Verify Azure CLI is installed and authenticated
- Check `az devops configure --list` for correct defaults
- Test the command manually first

## Getting Help

- Check existing commands for patterns to follow
- Review the README for user-facing documentation
- Open an issue for questions or problems
