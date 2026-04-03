# Atlassian MCP Server

Connects GitHub Copilot to Jira and Confluence via Atlassian's hosted MCP endpoint. No local process required — it runs over HTTP.

---

## What It Can Do

- Search and read Jira issues, epics, and sprints
- Create and update Jira issues
- Transition issue statuses
- Read and search Confluence pages and spaces

---

## Setup

### 1. Authorize Atlassian

This server uses Atlassian's own hosted MCP service at `https://mcp.atlassian.com`. Authentication is handled via OAuth in the browser.

1. Open Command Palette → `MCP: List Servers`
2. Find `atlassian-mcp-server` and click **Start**
3. VS Code will open a browser window asking you to sign in to Atlassian and grant access
4. After approval, the server is ready

> You must have an active Atlassian account with access to the Jira/Confluence sites you want to query.

### 2. Try It Out

Open Copilot Chat in Agent mode and try:

```
Show me Jira issues assigned to me
```

```
Find the Confluence page for our onboarding documentation
```

```
Create a Jira bug titled "Login fails on Safari" in project DEV
```

---

## Configuration (`.vscode/mcp.json`)

```jsonc
"atlassian-mcp-server": {
  "type": "http",
  "url": "https://mcp.atlassian.com/v1/mcp"
}
```

This is an HTTP-type server — no `command` or local package needed. Atlassian hosts it.

---

## Troubleshooting

- **Auth loop / keeps redirecting** — sign out of Atlassian in the browser and try again
- **Can't see certain projects** — your Atlassian account must have view permissions for those projects
- **Server shows as disconnected** — HTTP servers require a network connection; check your VPN/proxy settings
