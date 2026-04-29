---
name: opencode-add-api
description: "Integrate any external API into OpenCode by walking through documentation, authentication, and implementation. Use when the user wants to connect an API to OpenCode, says 'integrate this API', 'add an API', 'how do I use this API in OpenCode', 'connect to a service', or pastes API documentation and wants to use it."
user-invokable: true
disable-model-invocation: false
---

# Integrate an API into OpenCode

Walk through the full process of integrating any external API into OpenCode: identifying the right endpoint, reading the docs, getting an API key, and wiring it up securely.

## Workflow

### Step 1 — Determine the task

Ask the user:

> What do you want to do with this API? Describe the outcome you want (e.g. "send a message", "fetch data", "create a record").

This scopes the integration to a single function rather than the entire API surface.

### Step 2 — Check the documentation

Ask the user to provide one of:
- A link to the API documentation
- A paste of the relevant docs section
- A curl example from the docs

Then identify:

| What to find | Where to look |
|---|---|
| The specific endpoint for the task | Docs or curl example |
| Required parameters | Request body / query params section |
| Authentication method | Auth / Getting Started section |
| Response format | Response schema or example |

If the user hasn't found the right function yet, help them search the docs for it. Ask: "Does the API documentation have a section for [task]? Paste that section here."

### Step 3 — Copy the documentation into OpenCode

Once the relevant docs section is identified, instruct the user:

> Copy the documentation for this endpoint and paste it here. Include the endpoint URL, required parameters, and any example request/response.

Use the pasted documentation to draft the integration code or configuration.

### Step 4 — Get the API key

Ask the user where to find the API key for this service. Common locations:

- Dashboard → Settings → API Keys
- Developer Portal → Credentials
- Account → Integrations

Remind the user: **never paste the API key directly into the chat.** It will appear in the conversation history.

### Step 5 — Share the API key securely

Instruct the user to store the key in the repo `.env` file:

```bash
# Add to your .env file at the repo root
YOUR_SERVICE_API_KEY=your_key_here
```

Then reference it in code as:
```python
import os
api_key = os.getenv("YOUR_SERVICE_API_KEY")
```

Or in a shell script:
```bash
source .env
curl -H "Authorization: Bearer $YOUR_SERVICE_API_KEY" ...
```

**Never hardcode the key in a script or config file that gets committed.**

### Step 6 — Implement and test

Draft the minimal implementation for the task:
1. Write the API call using the documented endpoint and parameters
2. Load the API key from the environment
3. Run a dry test (read-only or sandbox if available) before any writes
4. Show the user the output and confirm it matches expectations

### Step 7 — Save as a reusable skill (optional)

If the user wants to reuse this integration, offer to wrap it as a skill:

> Want me to save this as a slash command skill so you can run it again with `/your-skill-name`?

If yes, use the `skill-creator` workflow to package it.

---

## Quick Reference

```
1. What do you want to do?          → scope the task
2. Find the right API function      → check the docs
3. Paste the docs into OpenCode     → let the LLM read it
4. Get your API key                 → from the platform dashboard
5. Store the key in .env            → never in code or chat
6. Test with a dry run first        → confirm it works
```
