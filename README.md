# Tech Fusion — MCP Setup Workshop

A hands-on guide for setting up **Model Context Protocol (MCP) servers** in VS Code with GitHub Copilot. Follow along step by step to connect Copilot to GitHub, Atlassian, SonarQube, PostgreSQL, and a persistent memory store.

---

## What is MCP?

**Model Context Protocol (MCP)** is an open standard that lets AI assistants (like GitHub Copilot) connect to external tools and data sources. Instead of copy-pasting context into chat, Copilot can directly query your databases, read your Jira tickets, inspect your SonarQube issues, and more — all through a standardized interface.

```
VS Code / Copilot  ──►  MCP Client  ──►  MCP Server  ──►  GitHub / Jira / DB / ...
```

---

## Prerequisites

Before you begin, make sure you have the following installed and ready:

| Requirement | Version | Notes |
|---|---|---|
| [VS Code](https://code.visualstudio.com/) | 1.99+ | MCP support requires a recent version |
| [Node.js](https://nodejs.org/) | 18+ | Required to run `npx`-based MCP servers |
| GitHub Copilot | Latest | Sign in to your GitHub account in VS Code |
| A GitHub Personal Access Token | — | [Create one here](https://github.com/settings/tokens) — needs `repo`, `read:org` scopes |

---

## Repository Structure

```
.
├── .vscode/
│   └── mcp.json          # MCP server definitions for this workspace
├── docs/
│   ├── github-mcp.md     # GitHub MCP server setup guide
│   ├── atlassian-mcp.md  # Atlassian (Jira/Confluence) setup guide
│   ├── sonarqube-mcp.md  # SonarQube setup guide
│   ├── postgres-mcp.md   # PostgreSQL setup guide
│   └── memory-mcp.md     # Persistent memory setup guide
├── .gitignore
└── README.md
```

---

## Quick Start

### 1. Clone this repository

```bash
git clone https://github.com/SyscoCorporation/tech-fusion-mcp-setup.git
cd tech-fusion-mcp-setup
```

### 2. Open in VS Code

```bash
code .
```

### 3. Open the MCP config

The `.vscode/mcp.json` file at the root of this repo defines all MCP servers. VS Code reads this automatically when you open the folder.

### 4. Start an MCP server

Open the Command Palette (`Cmd+Shift+P`) and run:

```
MCP: List Servers
```

Click **Start** next to any server. VS Code will prompt you for any required secrets (tokens, connection strings) — these are never stored in the file.

### 5. Use Copilot with MCP

Open a Copilot Chat panel (`Cmd+Ctrl+I`), switch to **Agent mode**, and try a prompt like:

```
List my open GitHub pull requests
```

---

## MCP Servers in This Repo

| Server | Purpose | Docs |
|---|---|---|
| `github-mcp-server` | Read/write GitHub repos, PRs, issues | [docs/github-mcp.md](docs/github-mcp.md) |
| `atlassian-mcp-server` | Query Jira issues and Confluence pages | [docs/atlassian-mcp.md](docs/atlassian-mcp.md) |
| `sonarqube-mcp-server` | Inspect code quality issues and metrics | [docs/sonarqube-mcp.md](docs/sonarqube-mcp.md) |
| `postgres-mcp-server` | Run read-only queries against a PostgreSQL DB | [docs/postgres-mcp.md](docs/postgres-mcp.md) |
| `memory-mcp-server` | Give Copilot a persistent memory across sessions | [docs/memory-mcp.md](docs/memory-mcp.md) |

---

## Understanding `.vscode/mcp.json`

Each entry in `servers` follows this shape:

```jsonc
"server-name": {
  "command": "npx",          // how to launch the server
  "args": ["-y", "package"], // the npm package to run
  "env": {
    "API_KEY": "${input:my_key}" // secrets — prompted at runtime, never stored
  },
  "type": "stdio"            // transport: "stdio" (local) or "http" (remote)
}
```

**Inputs** (defined separately) are prompted once per session and masked:

```jsonc
"inputs": [
  {
    "id": "my_key",
    "type": "promptString",
    "description": "Your API key",
    "password": true
  }
]
```

---

## Security Notes

- **Never commit tokens or secrets** to this repo. All credentials use `${input:...}` placeholders.
- The `memory.jsonl` file (created at runtime) is excluded by `.gitignore`.
- The PostgreSQL server runs in `READ_ONLY` mode — it cannot modify data.

---

## Resources

- [MCP Official Docs](https://modelcontextprotocol.io/)
- [VS Code MCP Support](https://code.visualstudio.com/docs/copilot/chat/mcp-servers)
- [MCP Server Registry](https://github.com/modelcontextprotocol/servers)