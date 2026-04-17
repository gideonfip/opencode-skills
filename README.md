# OpenCode Skills Repository

A collection of skills to automate the boring workflows in OpenCode. These skills help you:
- Add custom AI providers
- Manage MCP servers
- Organize your conversations

Note: If you are here from [freetiermodels.com](https://freetiermodels.com/), download and use the `opencode-add-provider` skill.

---

## What Are These Skills?

Think of these as **plugins** or **extensions** for OpenCode. Once installed, you can activate them by typing a `/` (slash) followed by the skill name.

**Example:**
- 
- Type `/opencode-mcp` to manage MCP servers
- Type `/opencode-sessions` to see your past conversations

---

## Quick Start for Beginners

### Prerequisites

You need:
1. **OpenCode** - The AI coding assistant (get it from [opencodes.dev](https://opencodes.dev))
2. **Terminal/Command Line** - Any terminal app on your computer

---

## Step-by-Step Installation

### Step 1: Download This Repository

You need to get the skill files onto your computer. Choose one method:

#### Option A: Using Git (Recommended if you have git installed)

1. Open your terminal
2. Navigate to where you want to save this:
   ```bash
   cd ~/Documents
   ```
3. Clone the repository:
   ```bash
   git clone https://github.com/your-username/opencode-skills.git
   ```

#### Option B: Download as ZIP

1. Go to the GitHub page for this repository
2. Click the green "Code" button
3. Click "Download ZIP"
4. Unzip the file to somewhere on your computer (like your Documents folder)

---

### Step 2: Install the Skills

Now you'll "teach" OpenCode about these skills by linking them to the right folder.

#### Where Do Skills Go?

OpenCode looks for skills in this folder on your computer:
```
~/.agents/skills/
```

(The `~` means "your home folder" - on Mac it's `/Users/yourname/`, on Linux it's `/home/yourname/`)

#### Installation Steps

1. **Open your terminal** (Terminal app on Mac, or Command Prompt/PowerShell on Windows)

2. **Create the skills folder** (if it doesn't exist):
   ```bash
   mkdir -p ~/.agents/skills
   ```

3. **Navigate to where you saved this repository**:
   
   If you used git (Option A):
   ```bash
   cd ~/Documents/opencode-skills
   ```
   
   If you downloaded the ZIP (Option B), navigate to wherever you unzipped it:
   ```bash
   cd ~/Documents/opencode-skills  # adjust path as needed
   ```

4. **Create symbolic links** (shortcuts) for each skill:
   
   ```bash
   ln -s "$(pwd)/skills/opencode-mcp" ~/.agents/skills/opencode-mcp
   ln -s "$(pwd)/skills/opencode-add-provider" ~/.agents/skills/opencode-add-provider
   ln -s "$(pwd)/skills/opencode-sessions" ~/.agents/skills/opencode-sessions
   ln -s "$(pwd)/skills/opencode-session-migrate" ~/.agents/skills/opencode-session-migrate
   ```
   
   **What this does:** Creates shortcuts in the `~/.agents/skills/` folder that point to the actual skill files.

---

### Step 3: Restart OpenCode

OpenCode needs to reload to see the new skills.

1. **Close** OpenCode completely
2. **Reopen** OpenCode
3. **Test it:** Type `/` (forward slash) in the chat box

You should see suggestions for the new skills!

---

## Copy & Paste Commands

Here are the complete commands you can copy and paste into your terminal:

### Install All Skills (One-Shot Commands)

```bash
# Create the skills directory
mkdir -p ~/.agents/skills

# Navigate to the repository folder (change this to where you cloned/downloaded it)
cd ~/Documents/opencode-skills

# Create links for all skills
ln -s "$(pwd)/skills/opencode-mcp" ~/.agents/skills/opencode-mcp
ln -s "$(pwd)/skills/opencode-add-provider" ~/.agents/skills/opencode-add-provider
ln -s "$(pwd)/skills/opencode-sessions" ~/.agents/skills/opencode-sessions
ln -s "$(pwd)/skills/opencode-session-migrate" ~/.agents/skills/opencode-session-migrate

echo "✓ Skills installed! Now restart OpenCode."
```

---

## Installing Individual Skills

Don't want all skills? Just install the ones you need:

### Only MCP Manager (`/opencode-mcp`)

```bash
mkdir -p ~/.agents/skills
ln -s "/path/to/opencode-skills/skills/opencode-mcp" ~/.agents/skills/opencode-mcp
```

**What it does:** Lets you add and manage MCP servers that extend OpenCode's capabilities.

### Only Add Provider (`/opencode-add-provider`)

```bash
mkdir -p ~/.agents/skills
ln -s "/path/to/opencode-skills/skills/opencode-add-provider" ~/.agents/skills/opencode-add-provider
```

**What it does:** Helps you add custom AI providers (other AI models) to OpenCode.

### Only Sessions (`/opencode-sessions`)

```bash
mkdir -p ~/.agents/skills
ln -s "/path/to/opencode-skills/skills/opencode-sessions" ~/.agents/skills/opencode-sessions
```

**What it does:** Manage your conversation history - view, resume, and export past chats.

### Only Session Migrate (`/opencode-session-migrate`)

```bash
mkdir -p ~/.agents/skills
ln -s "/path/to/opencode-skills/skills/opencode-session-migrate" ~/.agents/skills/opencode-session-migrate
```

**What it does:** Move your chat history when you rename or move a project folder.

---

## Troubleshooting

### "Command not found: ln"

If you're on **Windows**, the `ln` command might not work. Try using copy instead:

```bash
mkdir -p ~/.agents/skills
cp -r "C:\path\to\opencode-skills\skills\opencode-mcp" ~/.agents/skills/
cp -r "C:\path\to\opencode-skills\skills\opencode-add-provider" ~/.agents/skills/
cp -r "C:\path\to\opencode-skills\skills\opencode-sessions" ~/.agents/skills/
cp -r "C:\path\to\opencode-skills\skills\opencode-session-migrate" ~/.agents/skills/
```

### Skills don't appear after restart

1. Check the skills directory exists:
   ```bash
   ls ~/.agents/skills/
   ```
   
   You should see folders like `opencode-mcp`, `opencode-sessions`, etc.

2. Make sure the links point to actual files:
   ```bash
   ls -la ~/.agents/skills/
   ```
   
   You should see arrows (`->`) pointing to real files.

3. Try restarting OpenCode again (close completely and reopen)

### "No such file or directory"

Make sure:
- You're in the right folder where you cloned/downloaded the repo
- The paths in the `ln -s` command point to the actual skill folders

---

## How to Use These Skills

Once installed, you activate them by typing in OpenCode:

| Command | What it does |
|---------|--------------|
| `/opencode-mcp` | Add, configure, or debug MCP servers |
| `/opencode-add-provider` | Add new AI providers to OpenCode |
| `/opencode-sessions` | See and manage past conversations |
| `/opencode-session-migrate` | Move chat history when you move folders |

**Example usage:**

To add an MCP server, you might type:
```
/opencode-mcp Add the vidIQ MCP server
```

To see your past conversations:
```
/opencode-sessions List my sessions
```

---

## What's Inside This Repository?

```
opencode-skills/
├── skills/
│   ├── opencode-add-provider/     # Add custom AI providers
│   │   └── SKILL.md
│   ├── opencode-mcp/              # MCP server manager
│   │   └── SKILL.md
│   ├── opencode-sessions/         # Conversation history manager
│   │   └── SKILL.md
│   └── opencode-session-migrate/  # Move sessions between folders
│       └── SKILL.md
└── README.md                      # This file!
```

---

## Getting Help

If you're stuck:

1. **Check OpenCode's docs**: [opencodes.dev/docs](https://opencodes.dev/docs)
2. **Try the `/` command** in OpenCode - it will show available skills
3. **Check file paths** - make sure the repository folder path is correct

---

## For Your AI Assistant

To install these skills, paste this prompt into your AI assistant:

```
Please help me install the OpenCode skills from the repository at [YOUR_PATH_HERE].

I need to:
1. Create the ~/.agents/skills/ directory
2. Symlink these skill folders:
   - opencode-mcp
   - opencode-add-provider  
   - opencode-sessions
   - opencode-session-migrate

After installation, verify the skills are in place and tell me to restart OpenCode.
```

---

## Available Skills

### `/opencode-mcp` — MCP Manager

Manage Model Context Protocol (MCP) servers that extend OpenCode's capabilities.

**Use it when you want to:**
- Add a new MCP server
- Debug MCP connections
- Configure authentication
- List installed MCP servers

**Example:** "Add the vidIQ MCP server"

---

### `/opencode-add-provider` — Custom LLM Providers

Add custom OpenAI-compatible AI providers to OpenCode.

**Use it when you want to:**
- Connect to alternative AI models
- Use self-hosted LLMs
- Add providers not in the default list

**Example:** "Add a custom provider with this API endpoint"

Check out [freetiermodels.com](https://freetiermodels.com/) for all AI providers that offer free tiers.

---

### `/opencode-sessions` — Session Manager

Manage your conversation history, switch between past sessions, and export transcripts.

**Use it when you want to:**
- Find a previous conversation
- Resume work from last week
- Export chat history to a file
- Switch between projects

**Example:** "List my past sessions"

---

### `/opencode-session-migrate` — Session Migration

Move your chat history when you rename or relocate project folders.

**Use it when you want to:**
- Rename a project folder
- Move a project to a new location
- Relink sessions to the new path

**Example:** "I moved my project from /old/path to /new/path"
