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
export INTEGRACLAW_URL="http://localhost:8443"  # your server
```

### 1. Connect a Service

```bash
python <<'EOF'
import urllib.request, os, json
url = f'{os.environ["INTEGRACLAW_URL"]}/api/v1/connect/start'
data = json.dumps({"provider": "google", "service": "gmail"}).encode()
req = urllib.request.Request(url, data=data, method='POST')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
req.add_header('Content-Type', 'application/json')
result = json.load(urllib.request.urlopen(req))
print(f"Authorize here: {result['connect_url']}")
EOF
```

The user opens the URL, authorizes with Google/Microsoft/Notion, and the connection is active.

### 2. Use It

```bash
python <<'EOF'
import urllib.request, os, json
url = f'{os.environ["INTEGRACLAW_URL"]}/api/v1/action'
data = json.dumps({
    "provider": "google", "service": "gmail", "action": "send_email",
    "params": {"to": "team@company.com", "subject": "Weekly Report", "body": "Attached."}
}).encode()
req = urllib.request.Request(url, data=data, method='POST')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
req.add_header('Content-Type', 'application/json')
print(json.dumps(json.load(urllib.request.urlopen(req)), indent=2))
EOF
```

That's it. Token refresh, error handling, and API encoding are handled automatically.

## Action API

### Execute an Action

```
POST /api/v1/action
Authorization: Bearer $INTEGRACLAW_API_KEY
Content-Type: application/json

{
  "provider": "google",
  "service": "gmail",
  "action": "send_email",
  "params": { "to": "...", "subject": "...", "body": "..." },
  "connection_id": "optional — auto-resolved if omitted"
}
```

**Response:**
```json
{"success": true, "status": 200, "data": { ... }}
```

### Discover Actions

List all available actions with full JSON Schema:

```
GET /api/v1/tools
GET /api/v1/tools?app=google-gmail
```

Compact list (no schemas):

```
GET /api/v1/actions
GET /api/v1/actions?app=google-gmail
```

## Direct Token Access

When pre-built actions aren't enough, get a fresh OAuth token:

```
POST /api/v1/token/get
Authorization: Bearer $INTEGRACLAW_API_KEY
Content-Type: application/json

{"connection_id": "CONNECTION_ID"}
```

```json
{"access_token": "ya29.a0AfH6SM...", "expires_in": 3600, "token_type": "Bearer"}
```

Then call the native API directly:

```bash
python <<'EOF'
import urllib.request, os, json

# Get token
url = f'{os.environ["INTEGRACLAW_URL"]}/api/v1/token/get'
data = json.dumps({"connection_id": "CONNECTION_ID"}).encode()
req = urllib.request.Request(url, data=data, method='POST')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
req.add_header('Content-Type', 'application/json')
token = json.load(urllib.request.urlopen(req))["access_token"]

# Call Gmail directly
req = urllib.request.Request('https://gmail.googleapis.com/gmail/v1/users/me/messages?maxResults=5')
req.add_header('Authorization', f'Bearer {token}')
print(json.dumps(json.load(urllib.request.urlopen(req)), indent=2))
EOF
```

See [references/](references/) for native API guides per service.

## Connection Management

### Create

```
POST /api/v1/connect/start
{"provider": "google", "service": "gmail"}
```

Returns `connect_url` — open in browser to authorize. Optional `scopes` array to override defaults.

### Poll Status

```
GET /api/v1/connect/status/{session_id}
```

Returns `pending`, `completed`, `failed`, or `expired`. On `completed`, includes `connection_id`.

### Real-time Status (WebSocket)

```
ws://{host}/ws/connect/{session_id}?token=INTEGRACLAW_API_KEY
```

### List

```
GET /api/v1/connections
```

### Remove

```
DELETE /api/v1/connections/{connection_id}
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

### Send email, then log it to a spreadsheet

```python
import os, json, urllib.request

API = os.environ["INTEGRACLAW_URL"] + "/api/v1/action"
KEY = os.environ["INTEGRACLAW_API_KEY"]

def action(provider, service, act, params):
    data = json.dumps({"provider": provider, "service": service, "action": act, "params": params}).encode()
    req = urllib.request.Request(API, data=data, method='POST')
    req.add_header('Authorization', f'Bearer {KEY}')
    req.add_header('Content-Type', 'application/json')
    return json.load(urllib.request.urlopen(req))

# 1. Send email
action("google", "gmail", "send_email", {
    "to": "client@example.com",
    "subject": "Invoice #1234",
    "body": "Please find your invoice attached."
})

# 2. Log to spreadsheet
action("google", "sheets", "append_values", {
    "spreadsheet_id": "SHEET_ID",
    "range": "Log!A:C",
    "values": [["2026-03-08", "client@example.com", "Invoice #1234"]]
})
```

### Cross-platform: Google Calendar → Teams notification

```python
# 1. Get today's events
events = action("google", "calendar", "list_events", {
    "time_min": "2026-03-08T00:00:00Z",
    "time_max": "2026-03-08T23:59:59Z"
})

# 2. Post summary to Teams
summary = "\n".join(e["summary"] for e in events["data"].get("items", []))
action("microsoft", "teams", "send_channel_message", {
    "team_id": "TEAM_ID",
    "channel_id": "CHANNEL_ID",
    "content": f"Today's meetings:\n{summary}"
})
```

### Notion as a CRM: add contact from email

```python
# 1. Read email
email = action("google", "gmail", "read_message", {"message_id": "MSG_ID"})

# 2. Add to Notion database
action("notion", "databases", "query_database", {"database_id": "CRM_DB_ID"})
action("notion", "pages", "create_page", {
    "parent_id": "CRM_DB_ID",
    "properties": {
        "Name": {"title": [{"text": {"content": email["data"]["from"]}}]},
        "Email": {"email": email["data"]["from"]},
        "Source": {"select": {"name": "Inbound"}}
    }
})
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

If actions return 500 "token refresh failed", the OAuth grant was revoked. Delete and recreate the connection:

```bash
# DELETE /api/v1/connections/{id}
# POST /api/v1/connect/start {"provider": "google", "service": "gmail"}
# Open connect_url in browser
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

### curl

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"gmail","action":"list_messages","params":{"max_results":5}}'
```
