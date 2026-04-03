# SonarQube MCP Server

Connects GitHub Copilot to SonarQube so it can read code quality metrics, issues, hotspots, and quality gate statuses.

---

## What It Can Do

- List and search code quality issues by severity, type, or component
- Read security hotspots and their statuses
- Check quality gate pass/fail status for projects
- Pull code coverage and duplication metrics

---

## Setup

### 1. Create a SonarQube Personal Access Token

1. Log in to your SonarQube instance
2. Click your avatar (top right) → **My Account** → **Security**
3. Under **Generate Tokens**, enter a name (e.g. `copilot-mcp`) and click **Generate**
4. Copy the token — it will only be shown once

### 2. Start the Server in VS Code

1. Open Command Palette → `MCP: List Servers`
2. Find `sonarqube-mcp-server` and click **Start**
3. When prompted, paste your SonarQube token

### 3. Try It Out

Open Copilot Chat in Agent mode and try:

```
Show me critical SonarQube issues in the my-app project
```

```
What is the quality gate status for the backend service?
```

```
List all open security hotspots
```

---

## Configuration (`.vscode/mcp.json`)

```jsonc
"sonarqube-mcp-server": {
  "command": "npx",
  "args": ["-y", "sonarqube-mcp-server"],
  "env": {
    "SONARQUBE_URL": "https://your-sonarqube-instance.com/",
    "SONARQUBE_TOKEN": "${input:sonar_token}",
    "SONARQUBE_ORGANIZATION": ""
  }
}
```

| Field | Description |
|---|---|
| `SONARQUBE_URL` | Base URL of your SonarQube instance (include trailing slash) |
| `SONARQUBE_TOKEN` | Your PAT — prompted securely at runtime |
| `SONARQUBE_ORGANIZATION` | Leave empty for self-hosted; set for SonarCloud org |

---

## Troubleshooting

- **401 Unauthorized** — token is invalid or expired. Generate a new one.
- **No projects found** — verify `SONARQUBE_URL` is correct and your account has Browse permission on projects.
- **Server won't start** — run `node --version` to confirm Node.js 18+ is installed.
