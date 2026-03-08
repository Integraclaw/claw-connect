---
name: claw-connect
description: |
  Managed OAuth connector for AI agents. Call Google Workspace, Microsoft 365, and Notion through semantic actions — or grab a fresh token and hit native APIs directly.
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

Managed OAuth connector for AI agents. Two ways to work with external APIs:

1. **Semantic Actions** — call pre-built, validated operations like `send_email` or `create_event` with structured parameters. IntegraClaw handles tokens, refresh, and API quirks.
2. **Direct Token Access** — grab a fresh OAuth token and call native APIs yourself when actions don't cover your use case.

## How It Works

```
Your Agent                    IntegraClaw                   Third-Party API
    │                              │                              │
    ├── POST /action ─────────────►│                              │
    │   {provider, service,        │── refresh token ────────────►│
    │    action, params}           │◄── fresh access_token ───────│
    │                              │── call native API ──────────►│
    │◄── {success, data} ─────────│◄── response ─────────────────│
```

## Setup

```bash
export INTEGRACLAW_API_KEY="ic_YOUR_KEY"
export INTEGRACLAW_URL="http://localhost:8443"
```

### 1. Connect a Service

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/connect/start" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider": "google", "service": "gmail"}'
```

Response:
```json
{
  "session_id": "abc123-...",
  "connect_url": "http://localhost:8443/connect/gmail?session_token=...",
  "expires_in": 600
}
```

The user opens `connect_url` in a browser, authorizes with Google/Microsoft/Notion, and the connection is active.

### 2. Use It

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
    "action": "send_email",
    "params": {"to": "team@company.com", "subject": "Weekly Report", "body": "Attached."}
  }'
```

That's it. Token refresh, error handling, and API encoding are handled automatically.

## Action API

### Execute an Action

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
    "action": "send_email",
    "params": {"to": "...", "subject": "...", "body": "..."},
    "connection_id": "optional — auto-resolved if omitted"
  }'
```

**Response:**
```json
{"success": true, "status": 200, "data": { ... }}
```

### Discover Actions

List all available actions with full JSON Schema:

```bash
curl -s "$INTEGRACLAW_URL/api/v1/tools" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"

# Filter by app
curl -s "$INTEGRACLAW_URL/api/v1/tools?app=google-gmail" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

Compact list (no schemas):

```bash
curl -s "$INTEGRACLAW_URL/api/v1/actions" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

## Direct Token Access

When pre-built actions aren't enough, get a fresh OAuth token:

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/token/get" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"connection_id": "CONNECTION_ID"}'
```

```json
{"access_token": "ya29.a0AfH6SM...", "expires_in": 3600, "token_type": "Bearer"}
```

Then call the native API directly with the token:

```bash
# Get token
TOKEN=$(curl -s -X POST "$INTEGRACLAW_URL/api/v1/token/get" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"connection_id": "CONNECTION_ID"}' | jq -r '.access_token')

# Call Gmail API directly
curl -s "https://gmail.googleapis.com/gmail/v1/users/me/messages?maxResults=5" \
  -H "Authorization: Bearer $TOKEN"
```

See [references/](references/) for native API guides per service.

## Connection Management

### Create

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/connect/start" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider": "google", "service": "gmail"}'
```

Returns `connect_url` — open in browser to authorize. Optional `scopes` array to override defaults.

### Poll Status

```bash
curl -s "$INTEGRACLAW_URL/api/v1/connect/status/{session_id}" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

Returns `pending`, `completed`, `failed`, or `expired`. On `completed`, includes `connection_id`.

### Real-time Status (WebSocket)

```
ws://{host}/ws/connect/{session_id}?token=INTEGRACLAW_API_KEY
```

### List

```bash
curl -s "$INTEGRACLAW_URL/api/v1/connections" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

### Remove

```bash
curl -s -X DELETE "$INTEGRACLAW_URL/api/v1/connections/{connection_id}" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

### Multiple Connections

If you have multiple connections for the same service, pass `connection_id` in the action request. Otherwise IntegraClaw picks the first active one.

## Supported Services

### Google Workspace

| Service | Key | Actions |
|---------|-----|---------|
| Gmail | `google` / `gmail` | send_email, list_messages, search_messages, read_message, mark_as_read, archive, list_labels, create_draft |
| Calendar | `google` / `calendar` | list_events, get_event, create_event, update_event, delete_event, list_calendars, quick_add, check_availability |
| Drive | `google` / `drive` | list_files, search_files, get_file, download_file, create_folder, upload_file, share_file |
| Sheets | `google` / `sheets` | get_values, update_values, append_values, clear_values, get_spreadsheet, create_spreadsheet |

### Microsoft 365

| Service | Key | Actions |
|---------|-----|---------|
| Outlook | `microsoft` / `outlook` | send_email, list_messages, read_message, search_messages, create_draft |
| Calendar | `microsoft` / `calendar` | list_events, get_event, create_event, update_event, delete_event, list_calendars |
| Teams | `microsoft` / `teams` | list_teams, list_channels, send_channel_message, list_channel_messages |
| OneDrive | `microsoft` / `onedrive` | list_files, search_files, upload_file, download_file |

### Notion

| Service | Key | Actions |
|---------|-----|---------|
| Pages | `notion` / `pages` | search_pages, get_page, create_page, update_page |
| Databases | `notion` / `databases` | query_database, get_database, create_database |
| Blocks | `notion` / `blocks` | get_block_children, append_block_children, delete_block |

## Cookbook

### Send email via Gmail

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
    "action": "send_email",
    "params": {
      "to": "client@example.com",
      "subject": "Invoice #1234",
      "body": "Please find your invoice attached."
    }
  }'
```

### Create a Google Calendar event

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
      "start_time": "2026-03-10T10:00:00Z",
      "end_time": "2026-03-10T11:00:00Z",
      "description": "Weekly sync"
    }
  }'
```

### Read values from Google Sheets

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "sheets",
    "action": "get_values",
    "params": {"spreadsheet_id": "SPREADSHEET_ID", "range": "Sheet1!A1:D10"}
  }'
```

### Log to a spreadsheet after sending email

```bash
# 1. Send email
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google", "service": "gmail", "action": "send_email",
    "params": {"to": "client@example.com", "subject": "Invoice #1234", "body": "Hello!"}
  }'

# 2. Append row to tracking sheet
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google", "service": "sheets", "action": "append_values",
    "params": {
      "spreadsheet_id": "SHEET_ID",
      "range": "Log!A:C",
      "values": [["2026-03-08", "client@example.com", "Invoice #1234"]]
    }
  }'
```

### Send a Microsoft Teams message

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "microsoft",
    "service": "teams",
    "action": "send_channel_message",
    "params": {
      "team_id": "TEAM_ID",
      "channel_id": "CHANNEL_ID",
      "content": "Deploy complete!"
    }
  }'
```

### Query a Notion database

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "databases",
    "action": "query_database",
    "params": {"database_id": "DATABASE_ID"}
  }'
```

### Create a Notion page from email data

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "notion",
    "service": "pages",
    "action": "create_page",
    "params": {
      "parent_id": "CRM_DB_ID",
      "properties": {
        "Name": {"title": [{"text": {"content": "John Doe"}}]},
        "Email": {"email": "john@example.com"},
        "Source": {"select": {"name": "Inbound"}}
      }
    }
  }'
```

### List OneDrive files

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider": "microsoft", "service": "onedrive", "action": "list_files", "params": {}}'
```

## Error Reference

| Code | When | Fix |
|------|------|-----|
| 400 | Bad request, unknown action, missing params | Check action name and required params via `GET /api/v1/tools` |
| 401 | Invalid API key | Verify `INTEGRACLAW_API_KEY` is set and valid |
| 403 | Connection belongs to another key | Use a connection created with this API key |
| 404 | No active connection for service | Create one via `POST /api/v1/connect/start` |
| 502 | Upstream API error | Check `error` field — the third-party API returned an error |

Error response:
```json
{"success": false, "error": "no active connection found for google/gmail — connect via the dashboard"}
```

### Expired OAuth Token

If actions return 500 "token refresh failed", the OAuth grant was revoked. Delete and recreate:

```bash
# Remove old connection
curl -s -X DELETE "$INTEGRACLAW_URL/api/v1/connections/{id}" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"

# Create new one
curl -s -X POST "$INTEGRACLAW_URL/api/v1/connect/start" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider": "google", "service": "gmail"}'
# Open connect_url in browser to re-authorize
```

## Native API References

Detailed endpoint guides for direct API access (via token):

- [Gmail](references/google-mail/README.md) — Messages, threads, labels, drafts
- [Google Calendar](references/google-calendar/README.md) — Events, calendars, free/busy
- [Google Drive](references/google-drive/README.md) — Files, folders, uploads, sharing
- [Google Sheets](references/google-sheets/README.md) — Values, ranges, formatting
- [Outlook](references/outlook/README.md) — Mail, folders, drafts
- [Outlook Calendar](references/microsoft-calendar/README.md) — Events, attendees
- [Microsoft Teams](references/microsoft-teams/README.md) — Teams, channels, messages, chats
- [OneDrive](references/one-drive/README.md) — Files, folders, search, sharing
- [Notion](references/notion/README.md) — Pages, databases, blocks, search

## Language Examples

### JavaScript

```javascript
const res = await fetch(`${process.env.INTEGRACLAW_URL}/api/v1/action`, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${process.env.INTEGRACLAW_API_KEY}`
  },
  body: JSON.stringify({
    provider: 'google', service: 'gmail', action: 'send_email',
    params: { to: 'user@example.com', subject: 'Hi', body: 'Hello!' }
  })
});
const data = await res.json();
```

### Python

```python
import os, requests

resp = requests.post(
    f'{os.environ["INTEGRACLAW_URL"]}/api/v1/action',
    headers={'Authorization': f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}'},
    json={
        'provider': 'google', 'service': 'gmail', 'action': 'send_email',
        'params': {'to': 'user@example.com', 'subject': 'Hi', 'body': 'Hello!'}
    }
)
data = resp.json()
```

### Go

```go
payload, _ := json.Marshal(map[string]any{
    "provider": "google", "service": "gmail", "action": "send_email",
    "params": map[string]any{"to": "user@example.com", "subject": "Hi", "body": "Hello!"},
})
req, _ := http.NewRequest("POST", os.Getenv("INTEGRACLAW_URL")+"/api/v1/action", bytes.NewReader(payload))
req.Header.Set("Authorization", "Bearer "+os.Getenv("INTEGRACLAW_API_KEY"))
req.Header.Set("Content-Type", "application/json")
resp, _ := http.DefaultClient.Do(req)
```
