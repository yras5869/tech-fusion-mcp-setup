# GitHub MCP Server

Connects GitHub Copilot to GitHub so it can read repositories, pull requests, issues, and more — directly from chat.

---

## What It Can Do

- List and search repositories, branches, commits
- Read, create, and update issues and pull requests
- Search code across repos
- Read file contents from any branch

---

## Setup

### 1. Create a GitHub Personal Access Token

1. Go to [https://github.com/settings/tokens](https://github.com/settings/tokens)
2. Click **Generate new token (classic)**
3. Give it a descriptive name, e.g. `copilot-mcp`
4. Select these scopes:
   - `repo` — full repository access
   - `read:org` — read org membership (required for org repos)
5. Click **Generate token** and copy it immediately

> Keep this token safe — treat it like a password. It will not be shown again.

### 2. Start the Server in VS Code

1. Open Command Palette → `MCP: List Servers`
2. Find `github-mcp-server` and click **Start**
3. When prompted, paste your Personal Access Token

### 3. Try It Out

Open Copilot Chat in Agent mode and try:

```
List the open pull requests that are assigned to me
```

```
Show me the last 5 commits on the main branch of <repo-name>
```

---

## Configuration (`.vscode/mcp.json`)

```jsonc
"github-mcp-server": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-github"],
  "env": {
    "GITHUB_PERSONAL_ACCESS_TOKEN": "${input:token}"
  },
  "type": "stdio"
}
```

| Field | Description |
|---|---|
| `GITHUB_PERSONAL_ACCESS_TOKEN` | Your PAT — prompted securely at runtime |
| `GITHUB_OWNER` | Default org/user scope for queries |

---

## Troubleshooting

- **403 Forbidden** — your token is missing the required scopes. Regenerate it with `repo` and `read:org`.
- **Server won't start** — make sure Node.js 18+ is installed: `node --version`
- **No results from org repos** — confirm `GITHUB_OWNER` matches your org name exactly (case-sensitive).
