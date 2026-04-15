# OpenCode Skills Repository

A collection of specialized automation skills designed to extend the capabilities of OpenCode.

## Overview

OpenCode skills are instructional sets that enable the assistant to execute complex, multi-step workflows via slash commands. These skills extend the functionality of OpenCode, allowing for system management, external API integration, and the automation of software engineering tasks.

## Installation

Follow these procedures to integrate these skills into your OpenCode environment.

### 1. Prerequisites
OpenCode must be installed and configured on the local system.

### 2. Directory Configuration
OpenCode scans the `~/.agents/skills/` directory for available slash commands. Create this directory if it does not already exist:
```bash
mkdir -p ~/.agents/skills
```

### 3. Installation via Symlink
It is recommended to symlink the skills from this repository to the agents directory. This ensures that the local installation remains synchronized with updates to the repository.

**Example: Installing the `opencode-mcp` skill**
```bash
# Replace [REPO_PATH] with the absolute path to the cloned repository
ln -s [REPO_PATH]/skills/opencode-mcp ~/.agents/skills/opencode-mcp
```

### 4. Activation
1. **Restart OpenCode**: The application must be restarted to detect new skills in the directory.
2. **Verification**: Invoke the `/` command in the OpenCode interface to verify that the installed skills appear in the suggestions list.

---

## Available Skills

### `/opencode-mcp` — MCP Manager
Manages the lifecycle of Model Context Protocol (MCP) servers.
- **Function**: Automates the installation, configuration, and debugging of MCP servers.
- **Use Case**: Integrating remote servers (e.g., vidIQ for YouTube research) or local servers (e.g., Puppeteer for browser automation) without manual JSON editing.
- **Capabilities**: 
  - Configuration management in `~/.config/opencode/`
  - API key and OAuth integration
  - Connection diagnostics and health checks

### `/opencode-add-provider` — Custom LLM Providers
Facilitates the connection of OpenCode to OpenAI-compatible LLM providers.
- **Function**: Extracts base URLs and model identifiers from API documentation to automate provider configuration.
- **Use Case**: Integrating niche or self-hosted LLMs not included in the default provider list.

### `/opencode-sessions` — Session and History Manager
Provides advanced control over conversation context.
- **Function**: Enables the listing, resumption, and exportation of specific conversation sessions.
- **Use Case**: Managing multiple project contexts independently to maintain clean conversation history.

---

## Contributing

Contributions to the skills library are welcome. To contribute a new skill:

1. **Development**: Create the skill following the OpenCode specification (SKILL.md with appropriate YAML frontmatter).
2. **Validation**: Verify that the skill is correctly invoked via its slash command.
3. **Submission**: Submit a Pull Request adding the skill folder to the `skills/` directory.

## License
MIT
