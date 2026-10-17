# Meridian MCP server

Connect your AI tool to [Meridian](https://www.meridian4pm.com), the project management office (PMO)
workspace. Ask Claude, ChatGPT, Cursor or any MCP client about your projects, risks, status reports,
stakeholders and time, and let it log time or save a status report for you.

This is a hosted (remote) server. There is nothing to install or run.

```
https://www.meridian4pm.com/api/mcp
```

Use the `www` address exactly as shown. Full setup guide for each client:
**https://www.meridian4pm.com/mcp-server**

## Before you connect

- **A paid plan.** The MCP server is part of the Professional, Team and Enterprise plans.
- **An admin turns it on** under Settings, Integrations. Writes are a separate switch, off until an
  admin allows them.
- **Your role sets the limits.** Your AI tool acts as you. Viewers can read but never write. Guests
  cannot connect.

## Connect

**OAuth (recommended).** Add a remote MCP server (Streamable HTTP) with the address above. Your
client opens Meridian in the browser, you sign in, and you approve the connection. Every app is
offered the same access: everything your role and your organization allow, with the write
permissions ticked by default. You can untick them.

Claude Code:

```
claude mcp add --transport http --scope user meridian https://www.meridian4pm.com/api/mcp
```

Cursor, VS Code and similar clients (`mcp.json`):

```json
{
  "servers": {
    "meridian": { "type": "http", "url": "https://www.meridian4pm.com/api/mcp" }
  }
}
```

**Personal access token.** For a client without OAuth, create a token in Meridian under Settings,
Integrations and send it as a header:

```
Authorization: Bearer <your token>
```

## Tools

Read (scope `read:core`):

| Area | Tools |
| --- | --- |
| Projects and milestones | `list_projects`, `get_project`, `list_milestones` |
| Risks and change requests | `get_risk_register`, `list_change_requests` |
| Status reports | `get_status_report`, `get_status_report_context` (Admins and Scrum Masters) |
| Your time | `list_time_entries`, `get_timesheet_summary`, `list_time_projects`, `list_time_tags` |
| Team | `get_team_status` (Admins), `list_resources`, `get_my_access` |
| Stakeholders | `list_stakeholders`, `get_stakeholder` |
| Retrospectives | `list_retrospectives`, `get_retrospective`, `list_retro_action_items` |
| Access matrix | `get_access_matrix`, `get_access_review_status`, `get_access_entry_history` |

Write (need the organization's writes switch):

| Tool | Scope | Who |
| --- | --- | --- |
| `create_time_entry`, `update_time_entry` | `write:time-tracking` | Admins, Scrum Masters, Members |
| `create_status_report` | `write:status-reports` | Admins, Scrum Masters |

Writes can be previewed first, a retry never creates a duplicate, and edits refuse to overwrite a
change made since you last looked.

## Security

- Every request runs with your role and project access. If you cannot see something in Meridian,
  your AI tool cannot either.
- Disconnect an app or revoke a token under Settings, Integrations and it stops working at once.
  Changing your password also disconnects your connections.
- Every tool call is recorded against the connection that made it.
- Connections last up to 90 days. Anonymous retrospective cards never reveal their author.

## Support

Questions or problems: [www.meridian4pm.com/contact](https://www.meridian4pm.com/contact).

This repository holds the registry listing and documentation only. The server itself is part of
the Meridian service.
