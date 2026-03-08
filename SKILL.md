---
name: api-gateway
description: |
  Connect to Google Workspace, Microsoft 365, and Notion APIs with managed OAuth.
  Use this skill when users want to interact with external services like Gmail, Google Calendar, Google Drive, Google Sheets, Outlook, Microsoft Teams, OneDrive, or Notion.
  Security: The INTEGRACLAW_API_KEY authenticates with IntegraClaw but grants NO access to third-party services by itself. Each service requires explicit OAuth authorization by the user through IntegraClaw's connect flow. Access is strictly scoped to connections the user has authorized. Provided by IntegraClaw (https://integraclaw.ai).
compatibility: Requires network access and valid IntegraClaw API key
metadata:
  author: integraclaw
  version: "1.0"
  requires:
    env:
      - INTEGRACLAW_API_KEY
---

# API Gateway

Semantic action API and direct token access for third-party APIs using managed OAuth connections, provided by [IntegraClaw](https://integraclaw.ai). Execute pre-built actions or get fresh OAuth tokens for direct native API calls.

## Quick Start

```bash
# Execute an action: send an email via Gmail
python <<'EOF'
import urllib.request, os, json
data = json.dumps({
    "provider": "google",
    "service": "gmail",
    "action": "send_email",
    "params": {"to": "user@example.com", "subject": "Hello!", "body": "Hello from IntegraClaw!"}
}).encode()
req = urllib.request.Request('http://localhost:8443/api/v1/action', data=data, method='POST')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
req.add_header('Content-Type', 'application/json')
print(json.dumps(json.load(urllib.request.urlopen(req)), indent=2))
EOF
```

## Base URL

```
{INTEGRACLAW_BASE_URL}/api/v1
```

Default: `http://localhost:8443/api/v1`

## Authentication

All requests require the IntegraClaw API key in the Authorization header:

```
Authorization: Bearer $INTEGRACLAW_API_KEY
```

**Environment Variable:**

```bash
export INTEGRACLAW_API_KEY="ic_YOUR_API_KEY"
```

IntegraClaw automatically injects the appropriate OAuth token for the target service when executing actions.

## Getting Your API Key

1. Sign in to IntegraClaw dashboard
2. Go to **API Keys** section
3. Click **Create API Key** and copy the key (shown only once)

API keys start with the `ic_` prefix.

## Connection Management

### List Connections

```bash
python <<'EOF'
import urllib.request, os, json
req = urllib.request.Request('http://localhost:8443/api/v1/connections')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
print(json.dumps(json.load(urllib.request.urlopen(req)), indent=2))
EOF
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

### Create Connection

```bash
python <<'EOF'
import urllib.request, os, json
data = json.dumps({"provider": "google", "service": "gmail"}).encode()
req = urllib.request.Request('http://localhost:8443/api/v1/connect/start', data=data, method='POST')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
req.add_header('Content-Type', 'application/json')
print(json.dumps(json.load(urllib.request.urlopen(req)), indent=2))
EOF
```

**Request Body:**
- `provider` (required) - Provider name: `google`, `microsoft`, `notion`
- `service` (required) - Service name (see supported services table)
- `scopes` (optional) - Custom OAuth scopes (defaults to service defaults)

**Response:**
```json
{
  "session_id": "abc123-...",
  "connect_url": "http://localhost:8443/connect/gmail?session_token=...",
  "expires_in": 600
}
```

Open the returned `connect_url` in a browser to complete OAuth.

### Check Connection Status

```bash
python <<'EOF'
import urllib.request, os, json
req = urllib.request.Request('http://localhost:8443/api/v1/connect/status/{session_id}')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
print(json.dumps(json.load(urllib.request.urlopen(req)), indent=2))
EOF
```

**Response:**
```json
{
  "status": "completed",
  "connection_id": "21fd90f9-...",
  "email": "user@gmail.com"
}
```

Status values: `pending`, `completed`, `failed`, `expired`

### Delete Connection

```bash
python <<'EOF'
import urllib.request, os, json
req = urllib.request.Request('http://localhost:8443/api/v1/connections/{connection_id}', method='DELETE')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
print(json.dumps(json.load(urllib.request.urlopen(req)), indent=2))
EOF
```

### Specifying Connection

If you have multiple connections for the same service, specify which one to use via the `connection_id` field in the action request:

```json
{
  "provider": "google",
  "service": "gmail",
  "action": "send_email",
  "connection_id": "21fd90f9-5935-43cd-b6c8-bde9d915ca80",
  "params": {"to": "user@example.com", "subject": "Hello!", "body": "Hello!"}
}
```

If omitted, IntegraClaw uses the first active connection for that provider/service.

## Executing Actions

### Action API

```
POST /api/v1/action
```

**Request Body:**
```json
{
  "provider": "google",
  "service": "gmail",
  "action": "send_email",
  "connection_id": "optional-connection-id",
  "params": {
    "to": "user@example.com",
    "subject": "Hello",
    "body": "Message content"
  }
}
```

**Response:**
```json
{
  "success": true,
  "status": 200,
  "data": { ... }
}
```

### List Available Actions

Summary (without parameter schemas):
```bash
GET /api/v1/actions?app=google-gmail
```

Full detail (with JSON Schema parameters):
```bash
GET /api/v1/tools?app=google-gmail
```

### Get OAuth Token (Direct API Access)

For advanced use cases, get a fresh OAuth token to call native APIs directly:

```bash
python <<'EOF'
import urllib.request, os, json
data = json.dumps({"connection_id": "CONNECTION_ID"}).encode()
req = urllib.request.Request('http://localhost:8443/api/v1/token/get', data=data, method='POST')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
req.add_header('Content-Type', 'application/json')
result = json.load(urllib.request.urlopen(req))
print(result["access_token"])
EOF
```

**Response:**
```json
{
  "access_token": "ya29.a0AfH6SM...",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```

Use this token to call native APIs directly (see [references/](references/) for API guides).

## Supported Services

| Service | Provider | App Name | Actions Available |
|---------|----------|----------|-------------------|
| Gmail | `google` | `google-gmail` | Send, read, search, draft, reply, archive emails |
| Google Calendar | `google` | `google-calendar` | Create, list, update, delete events; check availability |
| Google Drive | `google` | `google-drive` | Upload, download, list, search, share files and folders |
| Google Sheets | `google` | `google-sheets` | Read, write, update spreadsheet data and formulas |
| Outlook | `microsoft` | `microsoft-outlook` | Send, read, search, draft, reply emails |
| Outlook Calendar | `microsoft` | `microsoft-calendar` | Create, list, update, delete calendar events |
| Microsoft Teams | `microsoft` | `microsoft-teams` | List teams, channels; send and read messages |
| OneDrive | `microsoft` | `microsoft-onedrive` | Upload, download, list, search files |
| Notion Pages | `notion` | `notion-pages` | Search, create, read, update pages |
| Notion Databases | `notion` | `notion-databases` | Query, create, update databases |
| Notion Blocks | `notion` | `notion-blocks` | Read, append, delete content blocks |

See [references/](references/) for detailed native API guides per provider:
- [Gmail](references/google-mail/README.md) - Messages, threads, labels, drafts
- [Google Calendar](references/google-calendar/README.md) - Events, calendars, free/busy
- [Google Drive](references/google-drive/README.md) - Files, folders, permissions, uploads
- [Google Sheets](references/google-sheets/README.md) - Values, ranges, formatting
- [Outlook](references/outlook/README.md) - Mail, calendar, contacts
- [Microsoft Calendar](references/microsoft-calendar/README.md) - Outlook Calendar events
- [Microsoft Teams](references/microsoft-teams/README.md) - Teams, channels, messages, chats
- [OneDrive](references/one-drive/README.md) - Files, folders, drives, sharing
- [Notion](references/notion/README.md) - Pages, databases, blocks, search

## Examples

### Gmail - Send Email

```bash
python <<'EOF'
import urllib.request, os, json
data = json.dumps({
    "provider": "google", "service": "gmail", "action": "send_email",
    "params": {"to": "recipient@example.com", "subject": "Hello!", "body": "Message from IntegraClaw"}
}).encode()
req = urllib.request.Request('http://localhost:8443/api/v1/action', data=data, method='POST')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
req.add_header('Content-Type', 'application/json')
print(json.dumps(json.load(urllib.request.urlopen(req)), indent=2))
EOF
```

### Google Calendar - Create Event

```bash
python <<'EOF'
import urllib.request, os, json
data = json.dumps({
    "provider": "google", "service": "calendar", "action": "create_event",
    "params": {
        "summary": "Team Meeting",
        "start_time": "2026-03-10T10:00:00Z",
        "end_time": "2026-03-10T11:00:00Z",
        "description": "Weekly sync"
    }
}).encode()
req = urllib.request.Request('http://localhost:8443/api/v1/action', data=data, method='POST')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
req.add_header('Content-Type', 'application/json')
print(json.dumps(json.load(urllib.request.urlopen(req)), indent=2))
EOF
```

### Google Sheets - Read Values

```bash
python <<'EOF'
import urllib.request, os, json
data = json.dumps({
    "provider": "google", "service": "sheets", "action": "get_values",
    "params": {"spreadsheet_id": "SPREADSHEET_ID", "range": "Sheet1!A1:D10"}
}).encode()
req = urllib.request.Request('http://localhost:8443/api/v1/action', data=data, method='POST')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
req.add_header('Content-Type', 'application/json')
print(json.dumps(json.load(urllib.request.urlopen(req)), indent=2))
EOF
```

### Microsoft Teams - Send Message

```bash
python <<'EOF'
import urllib.request, os, json
data = json.dumps({
    "provider": "microsoft", "service": "teams", "action": "send_channel_message",
    "params": {"team_id": "TEAM_ID", "channel_id": "CHANNEL_ID", "content": "Hello from IntegraClaw!"}
}).encode()
req = urllib.request.Request('http://localhost:8443/api/v1/action', data=data, method='POST')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
req.add_header('Content-Type', 'application/json')
print(json.dumps(json.load(urllib.request.urlopen(req)), indent=2))
EOF
```

### Notion - Query Database

```bash
python <<'EOF'
import urllib.request, os, json
data = json.dumps({
    "provider": "notion", "service": "databases", "action": "query_database",
    "params": {"database_id": "DATABASE_ID"}
}).encode()
req = urllib.request.Request('http://localhost:8443/api/v1/action', data=data, method='POST')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
req.add_header('Content-Type', 'application/json')
print(json.dumps(json.load(urllib.request.urlopen(req)), indent=2))
EOF
```

### Direct API Call (using token)

```bash
# Get token, then call native Gmail API directly
python <<'EOF'
import urllib.request, os, json

# Step 1: Get fresh OAuth token
data = json.dumps({"connection_id": "CONNECTION_ID"}).encode()
req = urllib.request.Request('http://localhost:8443/api/v1/token/get', data=data, method='POST')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
req.add_header('Content-Type', 'application/json')
token = json.load(urllib.request.urlopen(req))["access_token"]

# Step 2: Call native API
req = urllib.request.Request('https://gmail.googleapis.com/gmail/v1/users/me/messages?maxResults=5')
req.add_header('Authorization', f'Bearer {token}')
print(json.dumps(json.load(urllib.request.urlopen(req)), indent=2))
EOF
```

## Code Examples

### JavaScript (Node.js)

```javascript
const response = await fetch('http://localhost:8443/api/v1/action', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${process.env.INTEGRACLAW_API_KEY}`
  },
  body: JSON.stringify({
    provider: 'google',
    service: 'gmail',
    action: 'send_email',
    params: { to: 'user@example.com', subject: 'Hello!', body: 'Hello!' }
  })
});
```

### Python

```python
import os
import requests

response = requests.post(
    'http://localhost:8443/api/v1/action',
    headers={'Authorization': f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}'},
    json={
        'provider': 'google',
        'service': 'gmail',
        'action': 'send_email',
        'params': {'to': 'user@example.com', 'subject': 'Hello!', 'body': 'Hello!'}
    }
)
```

### Go

```go
payload := map[string]any{
    "provider": "google",
    "service":  "gmail",
    "action":   "send_email",
    "params": map[string]any{
        "to":      "user@example.com",
        "subject": "Hello!",
        "body":    "Hello!",
    },
}
body, _ := json.Marshal(payload)
req, _ := http.NewRequest("POST", "http://localhost:8443/api/v1/action", bytes.NewReader(body))
req.Header.Set("Authorization", "Bearer "+os.Getenv("INTEGRACLAW_API_KEY"))
req.Header.Set("Content-Type", "application/json")
resp, _ := http.DefaultClient.Do(req)
```

## Error Handling

| Status | Meaning |
|--------|---------|
| 400 | Invalid request, missing parameters, or unknown action |
| 401 | Invalid or missing API key |
| 403 | Connection does not belong to this API key |
| 404 | Connection not found or no active connection for service |
| 502 | Action failed (upstream API error) |
| 500 | Internal server error |

Error response format:
```json
{
  "success": false,
  "error": "description of the error"
}
```

### Troubleshooting: No Active Connection

If you get a 404 "no active connection found", create a connection first:

```bash
python <<'EOF'
import urllib.request, os, json
data = json.dumps({"provider": "google", "service": "gmail"}).encode()
req = urllib.request.Request('http://localhost:8443/api/v1/connect/start', data=data, method='POST')
req.add_header('Authorization', f'Bearer {os.environ["INTEGRACLAW_API_KEY"]}')
req.add_header('Content-Type', 'application/json')
result = json.load(urllib.request.urlopen(req))
print(f"Open this URL to authorize: {result['connect_url']}")
EOF
```

### Troubleshooting: Token Refresh Failed

A 500 "token refresh failed" may indicate an expired or revoked OAuth token. Delete the connection and create a new one:

1. Delete: `DELETE /api/v1/connections/{connection_id}`
2. Create: `POST /api/v1/connect/start`
3. Complete OAuth in browser

## WebSocket (Real-time Connection Status)

Monitor connection status in real-time:

```
ws://localhost:8443/ws/connect/{session_id}?token=INTEGRACLAW_API_KEY
```

Receives JSON messages when connection status changes:
```json
{"type": "connection.completed", "connection_id": "...", "email": "user@gmail.com"}
```

## Tips

1. **Use the action API**: Pre-built actions handle token refresh, error handling, and API quirks automatically.

2. **Direct API for advanced use**: Use `POST /api/v1/token/get` to get OAuth tokens for native API calls when actions don't cover your use case.

3. **Discover available actions**: `GET /api/v1/tools` returns all actions with full JSON Schema parameters.

4. **Connection reuse**: One connection per service is usually sufficient. Actions auto-resolve to the active connection.

5. **WebSocket for UX**: Use the WebSocket endpoint to show real-time connection status in UIs.
