# PostgreSQL MCP Server

Connects GitHub Copilot to a PostgreSQL database in **read-only** mode, allowing it to inspect schemas, run queries, and answer questions about your data.

---

## What It Can Do

- List databases, schemas, tables, and columns
- Run SELECT queries and return results
- Describe table structures and relationships
- Answer natural language questions about your data

> **Read-only enforced:** The server is configured with `READ_ONLY=true`. INSERT, UPDATE, DELETE, and DDL statements are blocked.

---

## Setup

### 1. Gather Your Connection String

You'll need a PostgreSQL connection string in this format:

```
postgresql://username:password@host:port/database
```

Example:
```
postgresql://myuser:secret@db.example.com:5432/mydb
```

Make sure the database user has at least `SELECT` privileges on the schemas you want to query.

### 2. Start the Server in VS Code

1. Open Command Palette → `MCP: List Servers`
2. Find `postgres-mcp-server` and click **Start**
3. When prompted, enter your full connection string

> The connection string is treated as a password input — it is masked and never stored.

### 3. Try It Out

Open Copilot Chat in Agent mode and try:

```
List all tables in the public schema
```

```
Show me the columns in the users table
```

```
How many orders were placed in the last 30 days?
```

---

## Configuration (`.vscode/mcp.json`)

```jsonc
"postgres-mcp-server": {
  "command": "npx",
  "args": [
    "-y",
    "@modelcontextprotocol/server-postgres",
    "${input:db_connection_string}"
  ],
  "env": {
    "READ_ONLY": "true"
  }
}
```

| Field | Description |
|---|---|
| `db_connection_string` | Full PostgreSQL connection URI — prompted at runtime |
| `READ_ONLY` | Restricts the server to SELECT queries only |

---

## Troubleshooting

- **Connection refused** — check firewall rules and that the host/port are reachable from your machine
- **Authentication failed** — verify username and password in the connection string
- **Permission denied on table** — grant SELECT on the specific schema/table to the DB user
- **SSL errors** — append `?sslmode=require` or `?sslmode=disable` to the connection string as needed
