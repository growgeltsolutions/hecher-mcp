# Hecher MCP Server

The official **Model Context Protocol (MCP)** server for [Hecher](https://hecher.app) — the multi-tenant CRM for Chabad shluchim by [Grow Gelt](https://hecher.app).

Connect Claude Desktop, ChatGPT, Cursor, or any other MCP‑compatible AI assistant to your Hecher account and let it search donors, record donations, run reports, and manage tasks for you — across all of your organizations (mosads).

> **Heads up:** This repository is the public listing and documentation for the Hecher MCP server. The server itself runs as a managed remote service at `https://api.hecher.app/mcp` — there is nothing to install or self-host.

---

## Quick start

1. In your MCP‑compatible client (Claude Desktop, ChatGPT, Cursor, etc.), add a new **remote MCP server** with this URL:

   ```
   https://api.hecher.app/mcp
   ```

2. The first time you use it, your browser will open and ask you to **sign in to Hecher** and approve the connection.
3. Once approved, the assistant can act on your account.

You can review and revoke connected assistants anytime under **Settings → AI Connections** inside Hecher.

---

## What it can do

The server exposes 20 tools that wrap the Hecher API. Every call runs **as you**, in whichever Mosad you've selected as active — RLS, billing gates, and audit logging all apply exactly as in the web app.

### Organizations (Mosads)
- `list_tenants` — list the Mosads you have access to
- `set_active_tenant` — choose which Mosad subsequent calls operate on
- `get_active_tenant` — show the currently active Mosad

### Donors
- `search_donors` — search by name, email, phone, tag, or status
- `get_donor` — full profile with stats and contact info
- `create_donor` / `update_donor` / `delete_donor`
- `add_donor_tag` / `remove_donor_tag` / `bulk_tag_donors`

### Donations
- `list_donations` — optionally filtered by donor or date
- `create_donation` / `update_donation` / `delete_donation`

### Activities & tasks
- `list_activities` — optionally filtered by donor, status, or assignee
- `create_activity` — task, call, meeting, or note
- `complete_activity`

### Reporting
- `get_dashboard_stats` — KPIs for the active Mosad
- `run_report` — execute a saved report

---

## Authentication

The server speaks **OAuth 2.1** with dynamic client registration, so any compliant MCP client can connect with no manual API keys.

| Endpoint | URL |
|---|---|
| Discovery | `https://api.hecher.app/.well-known/oauth-authorization-server` |
| Authorization | `https://api.hecher.app/oauth/authorize` |
| Token | `https://api.hecher.app/oauth/token` |
| Dynamic registration | `https://api.hecher.app/oauth/register` |

Each authorization issues an access token bound to **your user account** and the **Mosad you pick** during the consent flow. Tokens can be revoked at any time from **Settings → AI Connections**.

---

## Permissions & limits

- Tools act with the permissions of the signed‑in user in the active Mosad — no privilege escalation.
- All write operations honor Hecher's billing gates and audit log.
- Per‑user rate limits from the Hecher API gateway apply.
- Bulk mutations (e.g. `bulk_tag_donors`) flow through Hecher's background operation queue, the same as in the web UI.

---

## Compatible clients

Any MCP client that supports remote servers over Streamable HTTP with OAuth 2.1, including:

- [Claude Desktop](https://claude.ai/download)
- [ChatGPT](https://chat.openai.com)
- [Cursor](https://cursor.com)
- [Cline](https://github.com/cline/cline)
- [Continue](https://continue.dev)
- Any custom client built on the [MCP SDKs](https://modelcontextprotocol.io)

---

## Support

- **Product**: [hecher.app](https://hecher.app)
- **Issues with this listing or the MCP server**: open an issue in this repository
- **General Hecher support**: through the in‑app feedback button

---

## License

[MIT](./LICENSE) © Grow Gelt Solutions
