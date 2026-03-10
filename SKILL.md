---
name: claw-connect
description: |
  Connect to Google Workspace APIs (Gmail, Calendar, Drive, Sheets, Docs, Meet, YouTube, and more) with managed OAuth.
  Use this skill when users want to interact with Google services.
  Security: The INTEGRACLAW_API_KEY authenticates with Integraclaw but grants NO access to third-party services by itself. Each service requires explicit OAuth authorization by the user through Integraclaw's dashboard. Access is strictly scoped to connections the user has authorized. Provided by Integraclaw (https://integraclaw.dev).
compatibility: Requires network access and valid Integraclaw API key
metadata:
  author: integraclaw
  version: "2.1"
  homepage: "https://integraclaw.dev"
  requires:
    env:
      - INTEGRACLAW_URL
      - INTEGRACLAW_API_KEY
---

# Claw Connect

Semantic actions for Google Workspace APIs using managed OAuth connections, provided by [Integraclaw](https://integraclaw.dev). Integraclaw lets you call service actions through a single unified API.

IMPORTANT: Connections are managed by users through the Integraclaw dashboard. The agent's role is to **list available connections** and **call actions** using the references for each service. The agent never manages OAuth flows, tokens, or connection setup.

## Quick Start

```bash
# List available connections
curl -s "$INTEGRACLAW_URL/api/v1/connections" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"

# Call an action (see references for params)
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"gmail","action":"send_email","params":{"to":"user@example.com","subject":"Hello!","body":"Sent via Integraclaw"}}'
```

## Base URL

```
$INTEGRACLAW_URL/api/v1
```

## Authentication

All requests require the Integraclaw API key in the Authorization header:

```
Authorization: Bearer $INTEGRACLAW_API_KEY
```

**Environment variables:**

```bash
export INTEGRACLAW_URL="https://your-instance.com"
export INTEGRACLAW_API_KEY="ic_YOUR_KEY"
```

## How It Works

1. **User connects services** through the Integraclaw dashboard (OAuth or API key)
2. **Agent lists connections** via `GET /api/v1/connections` to see what's available
3. **Agent calls actions** via `POST /api/v1/action` — Integraclaw handles token management automatically
4. **Agent consults references** to know which actions exist and what params they accept

The agent never manages OAuth flows, tokens, or connection setup. That's all handled by Integraclaw.

## List Connections

Check which services the user has connected:

```bash
curl -s "$INTEGRACLAW_URL/api/v1/connections" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

**Response:**
```json
[
  {
    "id": "21fd90f9-...",
    "provider": "google",
    "service": "gmail",
    "email": "user@gmail.com",
    "status": "connected"
  }
]
```

Each connection has `provider`, `service`, `status`, and optionally `email`. Only use connections with `status: "connected"`.

You can filter by app:

```bash
curl -s "$INTEGRACLAW_URL/api/v1/connections?app=google-gmail&status=connected" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

## Action API

Execute actions through `POST /api/v1/action`:

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"PROVIDER","service":"SERVICE","action":"ACTION_NAME","params":{...}}'
```

**Request Body:**
- `provider` (required) — Provider name (see table below)
- `service` (required) — Service name (see table below)
- `action` (required) — Action name (see references for each service)
- `params` (required) — Action parameters (see references for each action)
- `connection_id` (optional) — Specific connection ID. If omitted, uses the first active connection for that provider/service.

**Response:**
```json
{
  "success": true,
  "status": 200,
  "data": { ... }
}
```

**Error Response:**
```json
{
  "success": false,
  "status": 400,
  "error": "missing required parameter: to"
}
```

## Discover Actions

List all available actions:

```bash
curl -s "$INTEGRACLAW_URL/api/v1/actions" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

Filter by app:

```bash
curl -s "$INTEGRACLAW_URL/api/v1/actions?app=google-gmail" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

List actions with full parameter schemas (MCP-compatible tools):

```bash
curl -s "$INTEGRACLAW_URL/api/v1/tools" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

## Supported Services

### Google Workspace

| Service | App Name | Actions |
|---------|----------|---------|
| Gmail | `google-gmail` | send_email, list_messages, search_messages, read_message, mark_as_read, archive, list_labels, create_draft |
| Calendar | `google-calendar` | list_calendars, list_events, search_events, get_event, create_event, update_event, delete_event, quick_add |
| Drive | `google-drive` | list_files, search_files, get_file, create_folder, copy_file, update_file, delete_file |
| Sheets | `google-sheets` | get_spreadsheet, read_range, write_range, append_rows, clear_range, create_spreadsheet |
| Docs | `google-docs` | create_document, get_document, insert_text, replace_text |
| Slides | `google-slides` | get_presentation, create_presentation, get_page, batch_update |
| Forms | `google-forms` | get_form, create_form, list_responses, get_response |
| Tasks | `google-tasks` | list_task_lists, create_task_list, delete_task_list, list_tasks, get_task, create_task, update_task, complete_task |
| Contacts | `google-contacts` | list_contacts, search_contacts, get_contact, create_contact, delete_contact |
| Meet | `google-meet` | create_space, get_space, end_conference, list_conference_records |
| YouTube | `google-youtube` | list_channels, search_videos, get_video, list_playlists, list_playlist_items |
| Search Console | `google-search-console` | list_sites, get_site, query_analytics, list_sitemaps |
| Analytics Admin | `google-analytics-admin` | list_accounts, list_properties, get_property, list_data_streams |
| Analytics Data | `google-analytics-data` | run_report, run_realtime_report, get_metadata |
| Ads | `google-ads` | search, list_customers, get_customer |
| Play | `google-play` | list_reviews, get_review, reply_review |
| Workspace Admin | `google-workspace-admin` | list_users, get_user, list_groups, get_group, list_group_members |

See [references/](references/) for detailed action guides per service:

- [Gmail](references/google-mail/README.md) — Send, list, search, read, draft
- [Google Calendar](references/google-calendar/README.md) — Events, calendars, quick add
- [Google Drive](references/google-drive/README.md) — Files, folders, search
- [Google Sheets](references/google-sheets/README.md) — Read, write, append, create
- [Google Docs](references/google-docs/README.md) — Create, read, insert text, find & replace
- [Google Slides](references/google-slides/README.md) — Presentations, pages, batch update
- [Google Forms](references/google-forms/README.md) — Forms, responses
- [Google Tasks](references/google-tasks/README.md) — Task lists, create, complete, subtasks
- [Google Contacts](references/google-contacts/README.md) — List, search, create, delete contacts
- [Google Meet](references/google-meet/README.md) — Create spaces, end conferences, records
- [Google YouTube](references/google-youtube/README.md) — Channels, videos, playlists
- [Google Search Console](references/google-search-console/README.md) — Sites, analytics, sitemaps
- [Google Analytics Admin](references/google-analytics-admin/README.md) — Accounts, properties, data streams
- [Google Analytics Data](references/google-analytics-data/README.md) — Reports, realtime, metadata
- [Google Ads](references/google-ads/README.md) — Search, customers
- [Google Play](references/google-play/README.md) — Reviews, replies
- [Google Workspace Admin](references/google-workspace-admin/README.md) — Users, groups, members

## Examples

### Gmail — Send Email

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"gmail","action":"send_email","params":{"to":"user@example.com","subject":"Weekly Report","body":"Please find the report attached.","cc":"manager@example.com"}}'
```

### Gmail — Search Messages

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"gmail","action":"search_messages","params":{"query":"from:boss@company.com newer_than:7d","max_results":5}}'
```

### Google Calendar — Create Event

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"calendar","action":"create_event","params":{"summary":"Team Meeting","start":"2026-03-10T10:00:00","end":"2026-03-10T11:00:00","timezone":"America/Sao_Paulo","attendees":["colleague@example.com"]}}'
```

### Google Sheets — Read Values

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"sheets","action":"read_range","params":{"spreadsheet_id":"SPREADSHEET_ID","range":"Sheet1!A1:D10"}}'
```

### Google Sheets — Append Rows

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"sheets","action":"append_rows","params":{"spreadsheet_id":"SPREADSHEET_ID","range":"Sheet1!A1","values":[["Name","Email","Status"],["John","john@example.com","Active"]]}}'
```

### Google Drive — List Files

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"drive","action":"list_files","params":{"page_size":20}}'
```

### Google Contacts — Search Contacts

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"contacts","action":"search_contacts","params":{"query":"John","page_size":10}}'
```

### Google Tasks — Create Task

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"tasks","action":"create_task","params":{"task_list_id":"TASK_LIST_ID","title":"Review document","notes":"Review the Q1 report","due":"2026-03-15T00:00:00Z"}}'
```

## Error Handling

| Status | Meaning | What to do |
|--------|---------|------------|
| 400 | Invalid request, missing params, or unknown action | Check the reference for correct action name and required params |
| 401 | Invalid or missing API key | Verify `INTEGRACLAW_API_KEY` is set correctly |
| 404 | No active connection for provider/service | Ask the user to connect the service through the Integraclaw dashboard |
| 4xx/5xx | Passthrough error from the target API | Check the `error` field for details from the third-party API |

### Troubleshooting

1. **Check env vars are set:**

```bash
echo "URL: $INTEGRACLAW_URL"
echo "KEY: ${INTEGRACLAW_API_KEY:0:10}..."
```

2. **Verify API key by listing connections:**

```bash
curl -s "$INTEGRACLAW_URL/api/v1/connections" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

3. **No connection for a service?** Ask the user to connect it through the Integraclaw dashboard. The agent cannot create connections.

## Tips

1. **Always list connections first** — before calling any action, check what services the user has connected.

2. **Use `connection_id` for disambiguation** — if the user has multiple connections for the same provider/service, specify which one to use.

3. **Consult references for params** — each service has a detailed reference in [references/](references/) with all actions and their parameters.

4. **Use `/api/v1/actions?app=APP_NAME`** to discover available actions for a specific service.

5. **Use `/api/v1/tools`** to get MCP-compatible tool schemas with full parameter definitions.

6. **Response data is raw** — the `data` field in the response contains the raw API response from the third-party service. Refer to the service's native API docs for response format details.

7. **One API for everything** — every action follows the same pattern: `POST /api/v1/action` with `provider`, `service`, `action`, `params`. No need to know different API endpoints.

## Notes

1. The `action` field in the request maps to the tool method as `{provider}_{service}_{action}`.
2. Integraclaw handles all OAuth token refresh automatically — the agent never deals with tokens.
3. Use `connection_id` only when the user has multiple connections for the same service.
4. If a 404 error occurs, the user needs to connect the service through the dashboard first. The agent cannot create connections.
5. All native API docs are still relevant for understanding response formats — Integraclaw returns the raw API response in the `data` field.
