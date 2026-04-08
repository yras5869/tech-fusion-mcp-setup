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

### Workshop Database (Supabase)

[Supabase](https://supabase.com) is used to demonstrate the Postgres MCP in this workshop because it offers a free hosted PostgreSQL database — no infrastructure setup required.

> In a real-world scenario, you would connect to your own application database wherever it is hosted (AWS RDS, Azure Database for PostgreSQL, on-premise, etc.). The setup steps are the same — just swap in your connection string.

**Connection method:** Use the **Session Pooler** connection string from your Supabase project.

To get it:
1. Go to your [Supabase dashboard](https://supabase.com/dashboard) → your project
2. Navigate to **Project Settings** → **Database**
3. Under **Connection string**, select the **Session pooler** tab
4. Copy the URI — it looks like:

```
postgresql://postgres.[project-ref]:[password]@aws-0-[region].pooler.supabase.com:5432/postgres
```

> Use the **Session pooler** (port `5432`) rather than the direct connection or transaction pooler, as it works best with tools like the MCP server that hold connections open.

---

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
