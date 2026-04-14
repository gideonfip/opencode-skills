# OpenCode Enhancement Skills Guide

This folder contains two skills to enhance your OpenCode experience:

## Skills Included

### 1. `/opencode-add-provider`
Add custom OpenAI-compatible LLM providers to OpenCode. Use when you need to connect to a provider not in the default list.

**How to use:**
1. Run `/opencode-add-provider`
2. Provide API documentation, curl example, or base URL and model details
3. Confirm the extracted configuration
4. Restart OpenCode and connect via `/connect` → "Other"

### 2. `/opencode-sessions`
Manage conversation sessions and history.

**Commands:**
- `/sessions` - List all sessions
- `/resume <id>` - Switch to session
- `/export` - Export current session

**Storage:** Conversations are stored in `~/.local/share/opencode/opencode.db`

## Installation

Place these SKILL.md files in your `.agents/skills/` directory or reference them from a central skills repository.

To use, simply invoke the skill with the `/skill-name` command in OpenCode.

## Configuration

No additional configuration required. Skills automatically use the standard OpenCode configuration paths.