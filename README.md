# OpenCode Skills Hub 🚀

An ecosystem of specialized automation skills designed to supercharge your OpenCode experience.

## Overview

OpenCode skills are "plugin" instructions that enable Claude to perform complex, multi-step workflows via slash commands. By adding these skills, you extend Claude's capabilities from simple chat to a powerful agent capable of managing your system, integrating external APIs, and automating repetitive engineering tasks.

## Installation Guide 🛠️

Follow these steps to integrate these skills into your OpenCode setup.

### 1. Prerequisites
Ensure you have [OpenCode](https://opencode.ai) installed and configured on your machine.

### 2. Set up the Skills Directory
OpenCode scans the `~/.agents/skills/` directory for available slash commands. If this directory doesn't exist, create it:
```bash
mkdir -p ~/.agents/skills
```

### 3. Install Skills via Symlink (Recommended)
Instead of copying files, we recommend symlinking the skills from this repository to your agents directory. This ensures that whenever you update the repository (`git pull`), your skills are updated automatically.

**Example: Installing the `opencode-mcp` skill**
```bash
# Replace [REPO_PATH] with the path where you cloned this repository
ln -s [REPO_PATH]/skills/opencode-mcp ~/.agents/skills/opencode-mcp
```

### 4. Verify and Activate
1. **Restart OpenCode**: The agent must be restarted to scan the directory for new skills.
2. **Test**: Type `/` in the OpenCode chat. You should now see the installed skills appearing in the suggestion list.

---

## Available Skills ⚡️

### `/opencode-mcp` — MCP Manager
The ultimate tool for extending Claude's capabilities via the **Model Context Protocol**.
- **What it does**: Handles the entire lifecycle of MCP servers—installation, configuration, and debugging.
- **Key Use Case**: Adding remote servers (like vidIQ for YouTube research) or local servers (like Puppeteer for browser automation) to your OpenCode environment without manually editing JSON files.
- **Capabilities**: 
  - Automatic config management in `~/.config/opencode/`
  - API key and OAuth integration
  - Connection debugging and health checks

### `/opencode-add-provider` — Custom LLM Providers
Easily connect OpenCode to any OpenAI-compatible LLM provider.
- **What it does**: Automates the extraction of base URLs and model IDs from API documentation to configure new providers.
- **Key Use Case**: Connecting to niche or self-hosted LLMs that aren't in the default provider list.

### `/opencode-sessions` — Session & History Manager
Advanced control over your conversation context.
- **What it does**: Allows you to list, resume, and export specific conversation sessions.
- **Key Use Case**: Switching between different project contexts without losing history or cluttering the current chat.

---

## Contributing 🤝

We welcome new skills that help the community! To contribute:

1. **Create your skill**: Follow the OpenCode skill format (SKILL.md with YAML frontmatter).
2. **Test it**: Ensure your skill is invocable via `/skill-name`.
3. **Submit**: Open a Pull Request adding your skill folder to the `skills/` directory.

## License
MIT
