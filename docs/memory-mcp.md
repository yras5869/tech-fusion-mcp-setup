# Memory MCP Server

Gives GitHub Copilot a **persistent memory** — a simple key-value / knowledge graph store that survives across sessions. Copilot can remember facts, preferences, and context you tell it to store.

---

## What It Can Do

- Store and retrieve named facts (entities and observations)
- Build a lightweight knowledge graph across sessions
- Remember project context, team conventions, or personal preferences
- Search stored memories by name or content

---

## Setup

### 1. Start the Server in VS Code

1. Open Command Palette → `MCP: List Servers`
2. Find `memory-mcp-server` and click **Start**

No credentials required. The server starts immediately.

### 2. Where Memory Is Stored

Memory is persisted to a local file at the root of this project:

```
tech-fusion-mcp-setup/memory.jsonl
```

This file is excluded from git via `.gitignore` — your stored memories stay local to your machine.

### 3. Try It Out

Open Copilot Chat in Agent mode and try:

```
Remember that our team uses Jira project key "DEV" for all backend work
```

```
What do you remember about our project conventions?
```

```
Forget everything you stored about the staging environment
```

---

## Configuration (`.vscode/mcp.json`)

```jsonc
"memory-mcp-server": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-memory"],
  "env": {
    "MEMORY_FILE_PATH": "memory.jsonl"
  },
  "type": "stdio"
}
```

| Field | Description |
|---|---|
| `MEMORY_FILE_PATH` | Path to the JSONL file where memories are persisted |

> To use an absolute path (recommended if you work from multiple directories), change the value to a full path like `/Users/yourname/memory.jsonl`.

---

## Troubleshooting

- **Memories lost after restart** — confirm `memory.jsonl` exists in the project root. If it's missing, the server will create it fresh on first write.
- **Server won't start** — run `node --version` to confirm Node.js 18+ is installed.
- **Want to reset memory** — delete `memory.jsonl` and restart the server.
