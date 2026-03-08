---
name: claw-connect
description: |
  Managed OAuth connector for AI agents. Call Google Workspace, Microsoft 365, and Notion through semantic actions or direct token access.
  Use when users want to send emails, manage calendars, read spreadsheets, upload files, query databases, post messages, or interact with any supported third-party service.
  The INTEGRACLAW_API_KEY authenticates with IntegraClaw but grants NO access to third-party services by itself. Each service requires explicit user authorization through the OAuth connect flow.
compatibility: Requires network access and valid IntegraClaw API key
metadata:
  author: integraclaw
  version: "1.0"
  requires:
    env:
      - INTEGRACLAW_API_KEY
---

# Claw Connect

Semantic actions and direct token access for third-party APIs using managed OAuth connections, provided by [IntegraClaw](https://integraclaw.ai).

Two modes:

- **Semantic Actions** — `POST /api/v1/action` with structured params. IntegraClaw handles token management, API encoding, and validation.
- **Direct Token Access** — `POST /api/v1/token/get` for a fresh OAuth token. Call native APIs yourself.

## Quick Start

```bash
export INTEGRACLAW_URL="http://localhost:8443"
export INTEGRACLAW_API_KEY="ic_YOUR_KEY"

# Send an email via semantic action
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
    "action": "send_email",
    "params": {
      "to": "recipient@example.com",
      "subject": "Hello!",
      "body": "Sent via IntegraClaw"
    }
  }'
```

## Base URL

```
$INTEGRACLAW_URL/api/v1
```

## Authentication

All requests require the IntegraClaw API key in the Authorization header:

```
Authorization: Bearer $INTEGRACLAW_API_KEY
```

## Connection Management

### Create Connection

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/connect/start" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider": "google", "service": "gmail"}'
```

**Request Body:**
- `provider` (required) — `google`, `microsoft`, or `notion`
- `service` (required) — See supported services table
- `scopes` (optional) — Custom OAuth scopes

**Response:**
```json
{
  "session_id": "abc123-...",
  "connect_url": "http://localhost:8443/connect/gmail?session_token=...",
  "expires_in": 600
}
```

Open the returned `connect_url` in a browser to complete OAuth.

### Get Connection Status

```bash
curl -s "$INTEGRACLAW_URL/api/v1/connect/status/{session_id}" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

**Response:**
```json
{
  "status": "completed",
  "connection_id": "21fd90f9-...",
  "email": "user@gmail.com"
}
```

### List Connections

```bash
curl -s "$INTEGRACLAW_URL/api/v1/connections" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

**Response:**
```json
[
  {
    "id": "21fd90f9-5935-43cd-b6c8-bde9d915ca80",
    "provider": "google",
    "service": "gmail",
    "email": "user@gmail.com",
    "scopes": ["https://www.googleapis.com/auth/gmail.readonly", "https://www.googleapis.com/auth/gmail.send"],
    "status": "connected",
    "connected_at": "2026-01-15T10:00:00Z"
  }
]
```

### Delete Connection

```bash
curl -s -X DELETE "$INTEGRACLAW_URL/api/v1/connections/{connection_id}" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

## Action API

Execute semantic actions through `POST /api/v1/action`:

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "PROVIDER",
    "service": "SERVICE",
    "action": "ACTION_NAME",
    "params": { ... }
  }'
```

**Request Body:**
- `provider` (required) — Provider name: `google`, `microsoft`, `notion`
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

### Discover Actions

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

List actions with full parameter schemas:

```bash
curl -s "$INTEGRACLAW_URL/api/v1/tools" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

## Direct Token Access

Get a fresh OAuth token to call native APIs yourself:

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/token/get" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"connection_id": "21fd90f9-5935-43cd-b6c8-bde9d915ca80"}'
```

**Response:**
```json
{
  "access_token": "ya29.a0AfH6SM...",
  "expires_in": 3599,
  "token_type": "Bearer"
}
```

Then call the native API directly:

```bash
TOKEN=$(curl -s -X POST "$INTEGRACLAW_URL/api/v1/token/get" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"connection_id": "CONNECTION_ID"}' | jq -r '.access_token')

curl -s "https://gmail.googleapis.com/gmail/v1/users/me/messages?maxResults=5" \
  -H "Authorization: Bearer $TOKEN"
```

## Supported Services

| Provider | Service | App Name | Actions |
|----------|---------|----------|---------|
| Google | Gmail | `google-gmail` | send_email, list_messages, search_messages, read_message, mark_as_read, archive, list_labels, create_draft |
| Google | Calendar | `google-calendar` | list_calendars, list_events, search_events, get_event, create_event, update_event, delete_event, quick_add |
| Google | Drive | `google-drive` | list_files, search_files, get_file, create_folder, copy_file, update_file, delete_file |
| Google | Sheets | `google-sheets` | get_spreadsheet, read_range, write_range, append_rows, clear_range, create_spreadsheet |
| Google | Tasks | `google-tasks` | list_task_lists, create_task_list, delete_task_list, list_tasks, get_task, create_task, update_task, complete_task |
| Google | Contacts | `google-contacts` | list_contacts, search_contacts, get_contact, create_contact, delete_contact |
| Google | Docs | `google-docs` | create_document, get_document, insert_text, replace_text |
| Google | Meet | `google-meet` | create_space, get_space, end_conference, list_conference_records |
| Microsoft | Outlook | `microsoft-outlook` | send_email, list_messages, search_messages, read_message, list_folders |
| Microsoft | Calendar | `microsoft-calendar` | list_events, get_event, create_event, delete_event |
| Microsoft | Teams | `microsoft-teams` | list_teams, list_channels, send_message, list_messages |
| Microsoft | OneDrive | `microsoft-onedrive` | list_files, search_files, get_file, create_folder |
| Notion | Pages | `notion-pages` | search, get, create, update |
| Notion | Databases | `notion-databases` | query, get, create |
| Notion | Blocks | `notion-blocks` | get_children, append, delete |

See [references/](references/) for detailed action guides per service:
- [Gmail](references/google-mail/README.md) — Send, list, search, read, draft
- [Google Calendar](references/google-calendar/README.md) — Events, calendars, quick add
- [Google Drive](references/google-drive/README.md) — Files, folders, search
- [Google Sheets](references/google-sheets/README.md) — Read, write, append, create
- [Google Tasks](references/google-tasks/README.md) — Task lists, create, complete, subtasks
- [Google Contacts](references/google-contacts/README.md) — List, search, create, delete contacts
- [Google Docs](references/google-docs/README.md) — Create, read, insert text, find & replace
- [Google Meet](references/google-meet/README.md) — Create spaces, end conferences, records
- [Outlook](references/outlook/README.md) — Send, list, search, read, folders
- [Microsoft Calendar](references/microsoft-calendar/README.md) — Events, create, delete
- [Microsoft Teams](references/microsoft-teams/README.md) — Teams, channels, messages
- [OneDrive](references/one-drive/README.md) — Files, folders, search
- [Notion](references/notion/README.md) — Pages, databases, blocks

## Examples

### Gmail — Send Email

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
    "action": "send_email",
    "params": {
      "to": "user@example.com",
      "subject": "Weekly Report",
      "body": "Please find the report attached.",
      "cc": "manager@example.com"
    }
  }'
```

### Gmail — Search Messages

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
    "action": "search_messages",
    "params": {
      "query": "from:boss@company.com newer_than:7d",
      "max_results": 5
    }
  }'
```

### Google Calendar — Create Event

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "calendar",
    "action": "create_event",
    "params": {
      "summary": "Team Meeting",
      "start": "2026-03-10T10:00:00",
      "end": "2026-03-10T11:00:00",
      "timezone": "America/Sao_Paulo",
      "attendees": ["colleague@example.com"]
    }
  }'
```

### Google Calendar — Quick Add

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "calendar",
    "action": "quick_add",
    "params": {
      "text": "Lunch with John tomorrow at noon"
    }
  }'
```

### Google Sheets — Read and Write

```bash
# Read values
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "sheets",
    "action": "read_range",
    "params": {
      "spreadsheet_id": "1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgVE2upms",
      "range": "Sheet1!A1:D10"
    }
  }'

# Append rows
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "sheets",
    "action": "append_rows",
    "params": {
      "spreadsheet_id": "1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgVE2upms",
      "range": "Sheet1!A1",
      "values": [["Name", "Email", "Status"], ["John", "john@example.com", "Active"]]
    }
  }'
```

### Outlook — Send Email

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "outlook",
    "action": "send_email",
    "params": {
      "to": "recipient@example.com",
      "subject": "Hello from IntegraClaw",
      "body": "<p>This is an <b>HTML</b> email.</p>",
      "body_type": "html"
    }
  }'
```

### Microsoft Teams — Send Channel Message

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "teams",
    "action": "send_message",
    "params": {
      "team_id": "TEAM_ID",
      "channel_id": "CHANNEL_ID",
      "message": "Hello from IntegraClaw!"
    }
  }'
```

### OneDrive — List Files

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "onedrive",
    "action": "list_files",
    "params": {
      "folder_path": "/Documents",
      "top": 10
    }
  }'
```

### Notion — Query Database

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "databases",
    "action": "query",
    "params": {
      "database_id": "DATABASE_ID",
      "filter": {"property": "Status", "select": {"equals": "Active"}},
      "page_size": 50
    }
  }'
```

### Notion — Create Page

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "pages",
    "action": "create",
    "params": {
      "parent_database_id": "DATABASE_ID",
      "title": "Meeting Notes",
      "properties": {
        "Status": {"select": {"name": "Active"}}
      }
    }
  }'
```

### Google Tasks — Create Task

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "tasks",
    "action": "create_task",
    "params": {
      "title": "Review pull request",
      "notes": "Check for security issues",
      "due": "2026-03-15T00:00:00Z"
    }
  }'
```

### Google Tasks — Complete Task

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "tasks",
    "action": "complete_task",
    "params": {
      "tasklist_id": "TASKLIST_ID",
      "task_id": "TASK_ID"
    }
  }'
```

### Google Contacts — Search

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "contacts",
    "action": "search_contacts",
    "params": {
      "query": "John"
    }
  }'
```

### Google Docs — Create and Write

```bash
# Create a new document
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "docs",
    "action": "create_document",
    "params": {
      "title": "Meeting Notes"
    }
  }'

# Insert text into the document
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "docs",
    "action": "insert_text",
    "params": {
      "document_id": "DOCUMENT_ID",
      "text": "Agenda:\n1. Project updates\n2. Action items\n"
    }
  }'
```

### Google Meet — Create Meeting

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "meet",
    "action": "create_space",
    "params": {}
  }'
```

### Cross-Service: Email + Spreadsheet

```bash
# Read data from Sheets
DATA=$(curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "sheets",
    "action": "read_range",
    "params": {"spreadsheet_id": "SHEET_ID", "range": "Sheet1!A1:C5"}
  }' | jq -r '.data.values | .[] | join(", ")' )

# Send it via email
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d "$(jq -n --arg body "$DATA" '{
    "provider": "google",
    "service": "gmail",
    "action": "send_email",
    "params": {"to": "boss@company.com", "subject": "Weekly Report Data", "body": $body}
  }')"
```

## Error Handling

| Status | Meaning |
|--------|---------|
| 400 | Invalid request, missing params, or unknown action |
| 401 | Invalid or missing API key |
| 404 | No active connection for provider/service |
| 502 | Action handler failed (target API error) |

### Troubleshooting: No Connection

```bash
# Create a connection first
curl -s -X POST "$INTEGRACLAW_URL/api/v1/connect/start" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider": "google", "service": "gmail"}'
# Open connect_url in browser to authorize
```

### Troubleshooting: Token Refresh Failed

Delete the connection and create a new one:

```bash
curl -s -X DELETE "$INTEGRACLAW_URL/api/v1/connections/{id}" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"

curl -s -X POST "$INTEGRACLAW_URL/api/v1/connect/start" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider": "google", "service": "gmail"}'
```

## WebSocket (Real-time Connection Status)

```
ws://{host}/ws/connect/{session_id}?token=INTEGRACLAW_API_KEY
```

## List Providers

```bash
curl -s "$INTEGRACLAW_URL/api/v1/providers" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

## Notes

1. The `action` field in the request maps to the tool method as `{provider}_{service}_{action}`.
2. IntegraClaw handles all OAuth token refresh automatically.
3. Use `connection_id` only when you have multiple connections for the same service.
4. All native API docs are still relevant for understanding response formats — IntegraClaw returns the raw API response in the `data` field.
