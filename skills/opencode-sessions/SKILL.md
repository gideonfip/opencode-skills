---
name: opencode-sessions
description: Manage OpenCode conversation history, sessions, and transcripts. Use when the user wants to view past conversations, list previous sessions, switch between sessions, export transcripts to markdown, or locate where conversation history is stored. Trigger phrases: "view past conversations", "list sessions", "resume session", "export transcript", "conversation history", "past chats", "switch session".
user-invokable: true
disable-model-invocation: false
---

# OpenCode Session Manager

Access and manage your OpenCode conversation history, switch between sessions, and export transcripts.

## Quick Commands

| Command | Purpose |
|---------|---------|
| `/sessions` or `/resume` | List all past sessions |
| `/resume <session-id>` | Switch to a specific session |
| `/export` | Export current session to markdown |

## Storage Location

OpenCode stores conversation history in a **SQLite database**:

```
~/.local/share/opencode/opencode.db
```

This is a single database file (typically 1-2 GB) containing:
- All conversation messages
- Session metadata
- Tool outputs and results

**Other relevant directories:**
| Path | Purpose |
|------|---------|
| `~/.local/share/opencode/log/` | Log files (last 10 sessions) |
| `~/.local/share/opencode/snapshot/` | Session snapshots |
| `~/.local/share/opencode/tool-output/` | Tool execution outputs |
| `~/.config/opencode/opencode.json` | Configuration file |

## Usage

### List All Sessions

```
/sessions
```

Shows a numbered list of recent sessions with:
- Session ID
- Date/time started
- Brief preview of first message

### Resume a Session

```
/resume 3
```

Switches to session #3 from the list. Your current context changes to that session's state.

**Tip:** You can also use `/resume` without a number to see the interactive list.

### Export Transcript

```
/export
```

Exports the current session transcript to markdown format for easier reading, sharing, or archiving.

**Output location:** Usually saved to your working directory or a default exports folder.

### Export Specific Session

```
/export <session-id>
```

Export a different session by specifying its ID.

## Workflow Examples

### Finding a Previous Solution

```
User: /sessions

OpenCode: 
1. 2026-04-03 10:15 - "Help me debug Python script"
2. 2026-04-03 14:22 - "Create React component"
3. 2026-04-04 09:00 - "Explain Git workflows"

User: /resume 2

OpenCode: [Switches to session about React component]
```

### Archiving Important Conversations

```
User: /export

OpenCode: Session exported to session_20260404_120000.md

User: [Move the file to your notes folder]
```

### Continuing Long-Term Work

```
User: /sessions

[Find the session from last week about the architecture design]

User: /resume 5

[Continue the architecture discussion with full context preserved]
```

## Best Practices

1. **Name your sessions implicitly** — The first message often becomes the session identifier. Start with a clear description like "Architecture review for authentication system"

2. **Export before closing** — If a session has valuable information, `/export` it before starting a new topic

3. **Resume for context** — Switching sessions preserves the full conversation state, including file changes and tool outputs

4. **Database maintenance** — The `opencode.db` file grows over time. If it gets too large, you can:
   - Export important sessions to markdown for archiving
   - Use `/sessions` and `/export` to selectively save what matters

## Troubleshooting

### Session not found
- Check `/sessions` to see available session IDs
- Sessions may be cleared if the database was reset

### Export fails
- Ensure you have write permissions in the target directory
- Try specifying a full path: `/export /path/to/output.md`

### Database issues
- If `~/.local/share/opencode/opencode.db` is corrupted, OpenCode may create a new one
- Check `~/.local/share/opencode/snapshot/` for recent session backups

---

**Related:** Use `/core-reflect` at session end to capture learnings from important conversations.
