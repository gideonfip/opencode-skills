---
name: opencode-add-provider
description: Add custom OpenAI-compatible LLM providers to OpenCode config. Use when the user wants to add a new AI provider/model that isn't listed in the built-in /connect options. Trigger phrases: "add provider", "add custom provider", "add model to opencode", "new provider", "configure provider", "add AI provider".
user-invokable: true
disable-model-invocation: false
---

# Add Custom Provider to OpenCode

Add any OpenAI-compatible LLM provider to the OpenCode configuration by parsing documentation or curl examples the user provides.

## Config File Location

```
~/.config/opencode/opencode.json
```

## Workflow

### Step 1: Ask for Documentation

Ask the user:

> Paste the API documentation, curl example, or any info you have about the provider you want to add. This could be a curl command, a docs page excerpt, or just the base URL and model name.

Wait for the user to provide one of:
- A curl command (like `curl -X POST "https://api.example.com/v1/chat/completions" ...`)
- API documentation text
- A URL to the provider's docs page
- Just the base URL and model details

### Step 2: Extract Configuration

From the provided information, extract:

| Field | Required | How to Find It |
|-------|----------|----------------|
| **Provider ID** | Yes | Short lowercase key, derived from the provider name (e.g. `modal`, `together`, `cerebras`) |
| **Display Name** | Yes | Human-readable provider name |
| **Base URL** | Yes | From curl endpoint or docs. Must end before `/chat/completions` — typically ends with `/v1` |
| **Model ID** | Yes | The model identifier the API expects (e.g. `zai-org/GLM-5-FP8`) |
| **Model Display Name** | Yes | Friendly name for the `/models` picker |
| **npm package** | No | Default: `@ai-sdk/openai-compatible`. Use `@ai-sdk/openai` only if the API uses `/v1/responses` instead of `/v1/chat/completions` |
| **Custom headers** | No | Only if the provider requires extra headers beyond `Authorization: Bearer` |
| **Additional models** | No | If the user mentions multiple models, add all of them |

#### Extraction Rules

1. **Base URL from curl**: Take the URL up to and including `/v1`. Remove `/chat/completions` and anything after.
   - `https://api.us-west-2.modal.direct/v1/chat/completions` → `https://api.us-west-2.modal.direct/v1`
   - `https://api.example.com/v1` → `https://api.example.com/v1`

2. **Provider ID**: Generate from the domain or brand name. Lowercase, hyphens for spaces.
   - `modal.direct` → `modal`
   - `Together AI` → `together-ai`
   - `api.fireworks.ai` → `fireworks`

3. **Model ID**: Use exactly what the API expects. From `-d '{"model": "..."}'` in curl or from docs.

4. **Multiple models**: If docs list several models, include all of them in the `models` block.

### Step 3: Confirm with User

Before making any changes, present the extracted config and ask for confirmation:

```
I extracted the following from your documentation:

Provider ID: modal
Display Name: Modal
Base URL: https://api.us-west-2.modal.direct/v1

Models:
  - zai-org/GLM-5-FP8 → "GLM-5 FP8"

This will be added to ~/.config/opencode/opencode.json

Does this look correct? Any changes needed?
```

Wait for the user to confirm or request changes.

### Step 4: Update Config

Read the current config at `~/.config/opencode/opencode.json`, then add the new provider to the `provider` block.

**Config template (with API key placeholder):**

```json
"<provider-id>": {
  "npm": "@ai-sdk/openai-compatible",
  "name": "<Display Name>",
  "options": {
    "baseURL": "<base-url>",
    "apiKey": "<YOUR_API_KEY>"
  },
  "models": {
    "<model-id>": {
      "name": "<Model Display Name>"
    }
  }
}
```

**If custom headers are needed:**

```json
"<provider-id>": {
  "npm": "@ai-sdk/openai-compatible",
  "name": "<Display Name>",
  "options": {
    "baseURL": "<base-url>",
    "apiKey": "<YOUR_API_KEY>",
    "headers": {
      "X-Custom-Header": "value"
    }
  },
  "models": {
    "<model-id>": {
      "name": "<Model Display Name>"
    }
  }
}
```

**Important:**
- Always include `"apiKey": "<YOUR_API_KEY>"` in `options` — this embeds the key directly in the config so the user does NOT need to run `/connect`
- Preserve all existing providers and other config (mcp, etc.)
- Add a comma after the previous provider's closing `}` before inserting the new one
- Do not modify or remove any existing entries

### Step 5: Next Steps

After updating the config, tell the user:

**Primary method — use `/connect`:**

1. Restart OpenCode (quit and relaunch)
2. Run `/connect` in OpenCode
3. Scroll down and select **Other**
4. Enter the provider ID (e.g. `modal`)
5. Paste your API key
6. Run `/models` to see and select the new model

**Fallback — if `/connect` doesn't work or the provider doesn't appear after restarting:**

The config already includes `"apiKey": "<YOUR_API_KEY>"` as a fallback. If the `/connect` method fails:

1. Open `~/.config/opencode/opencode.json`
2. Replace `<YOUR_API_KEY>` with your actual API key
3. Save the file
4. Restart OpenCode and run `/models` to select the new model

Always suggest `/connect` first. Only mention the manual API key fallback if the user reports issues.

## Example: Full Flow

**User provides:**

```bash
curl -X POST "https://api.us-west-2.modal.direct/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer sk-xxx" \
  -d '{"model": "zai-org/GLM-5-FP8", "messages": [{"role": "user", "content": "Hello"}]}'
```

**Extracted config:**

```json
"modal": {
  "npm": "@ai-sdk/openai-compatible",
  "name": "Modal",
  "options": {
    "baseURL": "https://api.us-west-2.modal.direct/v1",
    "apiKey": "<YOUR_API_KEY>"
  },
  "models": {
    "zai-org/GLM-5-FP8": {
      "name": "GLM-5 FP8"
    }
  }
}
```

**Result:** Added to `~/.config/opencode/opencode.json` under `provider`. User runs `/connect` → Other → `modal` → paste API key → `/models`. If that fails, user manually replaces `<YOUR_API_KEY>` in the config file.
