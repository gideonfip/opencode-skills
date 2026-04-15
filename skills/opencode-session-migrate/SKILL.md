---
name: opencode-session-migrate
description: "Migrate OpenCode sessions and projects from one directory to another. Use this skill whenever a user moves a project folder, renames a project directory, or wants to link existing chat history to a new directory path."
user-invokable: true
disable-model-invocation: false
---

# OpenCode Session Migration

This skill allows you to migrate project and session metadata in the OpenCode database when a project's filesystem path changes.

## Workflow

1. **Identify Paths**
   - Determine the **old path** (the original directory where the project was located).
   - Determine the **new path** (the current absolute path of the project directory).

2. **Update Project Worktree**
   Update the `project` table to reflect the new location.
   ```bash
   sqlite3 ~/.local/share/opencode/opencode.db "UPDATE project SET worktree = '[new-path]' WHERE worktree = '[old-path]';"
   ```

3. **Update Session Directories**
   Update all sessions associated with that project to point to the new directory.
   ```bash
   sqlite3 ~/.local/share/opencode/opencode.db "UPDATE session SET directory = '[new-path]' WHERE project_id = (SELECT id FROM project WHERE worktree = '[new-path]');"
   ```

## Verification
After migration, the user can verify the change by listing sessions or attempting to resume a session from the new directory.

- Run `/sessions` to ensure the project is recognized.
- Use `/resume <session-id>` to verify the context loads correctly from the new path.
