---
name: claw-connect
description: |
  Managed OAuth connector for AI agents. Call Google Workspace, Microsoft 365, and Notion through a gateway proxy or semantic actions.
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

Gateway proxy and semantic actions for third-party APIs using managed OAuth connections, provided by [IntegraClaw](https://integraclaw.ai). Call native API endpoints directly or use pre-built actions.

## Quick Start

```bash
# Native Gmail API call through IntegraClaw gateway
curl -s "$INTEGRACLAW_URL/gateway/google-mail/gmail/v1/users/me/messages?maxResults=5" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

## Base URL

```
$INTEGRACLAW_URL/gateway/{app}/{native-api-path}
```

Replace `{app}` with the service name and `{native-api-path}` with the actual API endpoint path.

IMPORTANT: The URL path MUST start with the connection's app name (eg. `/google-mail/...`). This prefix tells the gateway which app connection to use. For example, the native Gmail API path starts with `gmail/v1/`, so full paths look like `/gateway/google-mail/gmail/v1/users/me/messages`.

## Authentication

All requests require the IntegraClaw API key in the Authorization header:

```
Authorization: Bearer $INTEGRACLAW_API_KEY
```

**Environment Variable:**

```bash
export INTEGRACLAW_API_KEY="ic_YOUR_KEY"
export INTEGRACLAW_URL="http://localhost:8443"
```

The gateway automatically injects the appropriate OAuth token for the target service.

## Connection Management

Connection management uses the API base URL: `$INTEGRACLAW_URL/api/v1`

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

### Create Connection

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/connect/start" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider": "google", "service": "gmail"}'
```

**Request Body:**
- `provider` (required) - Provider name: `google`, `microsoft`, `notion`
- `service` (required) - Service name (see supported services table)
- `scopes` (optional) - Custom OAuth scopes

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

### Delete Connection

```bash
curl -s -X DELETE "$INTEGRACLAW_URL/api/v1/connections/{connection_id}" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

### Specifying Connection

If you have multiple connections for the same app, specify which connection to use by adding the `X-Connection` header with the connection ID:

```bash
curl -s "$INTEGRACLAW_URL/gateway/google-mail/gmail/v1/users/me/messages?maxResults=5" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "X-Connection: 21fd90f9-5935-43cd-b6c8-bde9d915ca80"
```

If omitted, the gateway uses the default (first) active connection for that app.

## Supported Services

| Service | App Name | Base URL Proxied |
|---------|----------|------------------|
| Gmail | `google-mail` | `gmail.googleapis.com` |
| Google Calendar | `google-calendar` | `www.googleapis.com` |
| Google Drive | `google-drive` | `www.googleapis.com` |
| Google Sheets | `google-sheets` | `sheets.googleapis.com` |
| Outlook | `outlook` | `graph.microsoft.com` |
| Microsoft Teams | `microsoft-teams` | `graph.microsoft.com` |
| OneDrive | `one-drive` | `graph.microsoft.com` |
| Notion | `notion` | `api.notion.com` |

See [references/](references/) for detailed routing guides per provider:
- [Gmail](references/google-mail/README.md) - Messages, threads, labels, drafts
- [Google Calendar](references/google-calendar/README.md) - Events, calendars, free/busy
- [Google Drive](references/google-drive/README.md) - Files, folders, permissions, uploads
- [Google Sheets](references/google-sheets/README.md) - Values, ranges, formatting
- [Outlook](references/outlook/README.md) - Mail, calendar, contacts
- [Microsoft Teams](references/microsoft-teams/README.md) - Teams, channels, messages, chats, meetings
- [OneDrive](references/one-drive/README.md) - Files, folders, drives, sharing
- [Notion](references/notion/README.md) - Pages, databases, blocks, search

## Examples

### Gmail - List Unread Messages

```bash
curl -s "$INTEGRACLAW_URL/gateway/google-mail/gmail/v1/users/me/messages?q=is:unread&maxResults=10" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

### Gmail - Send Message

```bash
curl -s -X POST "$INTEGRACLAW_URL/gateway/google-mail/gmail/v1/users/me/messages/send" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"raw": "BASE64_ENCODED_EMAIL"}'
```

### Google Calendar - List Events

```bash
curl -s "$INTEGRACLAW_URL/gateway/google-calendar/calendar/v3/calendars/primary/events?maxResults=10&orderBy=startTime&singleEvents=true" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

### Google Calendar - Create Event

```bash
curl -s -X POST "$INTEGRACLAW_URL/gateway/google-calendar/calendar/v3/calendars/primary/events" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "summary": "Team Meeting",
    "start": {"dateTime": "2026-03-10T10:00:00", "timeZone": "America/Sao_Paulo"},
    "end": {"dateTime": "2026-03-10T11:00:00", "timeZone": "America/Sao_Paulo"}
  }'
```

### Google Sheets - Get Values

```bash
curl -s "$INTEGRACLAW_URL/gateway/google-sheets/v4/spreadsheets/SPREADSHEET_ID/values/Sheet1!A1:D10" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

### Google Drive - List Files

```bash
curl -s "$INTEGRACLAW_URL/gateway/google-drive/drive/v3/files?pageSize=10&fields=files(id,name,mimeType,size)" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

### Outlook - Send Email

```bash
curl -s -X POST "$INTEGRACLAW_URL/gateway/outlook/v1.0/me/sendMail" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "message": {
      "subject": "Hello",
      "body": {"contentType": "Text", "content": "Hello from IntegraClaw!"},
      "toRecipients": [{"emailAddress": {"address": "recipient@example.com"}}]
    },
    "saveToSentItems": true
  }'
```

### Microsoft Teams - Send Channel Message

```bash
curl -s -X POST "$INTEGRACLAW_URL/gateway/microsoft-teams/v1.0/teams/{team-id}/channels/{channel-id}/messages" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"body": {"content": "Hello from IntegraClaw!"}}'
```

### OneDrive - List Files

```bash
curl -s "$INTEGRACLAW_URL/gateway/one-drive/v1.0/me/drive/root/children" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

### Notion - Search Pages

```bash
curl -s -X POST "$INTEGRACLAW_URL/gateway/notion/v1/search" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Notion-Version: 2022-06-28" \
  -d '{"query": "meeting notes", "filter": {"property": "object", "value": "page"}}'
```

### Notion - Query Database

```bash
curl -s -X POST "$INTEGRACLAW_URL/gateway/notion/v1/databases/{databaseId}/query" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Notion-Version: 2022-06-28" \
  -d '{"filter": {"property": "Status", "select": {"equals": "Active"}}, "page_size": 100}'
```

## Semantic Actions (Alternative)

IntegraClaw also provides a semantic action API with pre-built, validated operations:

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "gmail",
    "action": "send_email",
    "params": {"to": "user@example.com", "subject": "Hello!", "body": "Sent via IntegraClaw"}
  }'
```

List all available actions:

```bash
curl -s "$INTEGRACLAW_URL/api/v1/tools" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

Actions handle token refresh, parameter validation, and API encoding automatically.

## Error Handling

| Status | Meaning |
|--------|---------|
| 400 | Missing connection for the requested app |
| 401 | Invalid or missing API key |
| 404 | No active connection for the app |
| 500 | Internal server error |
| 4xx/5xx | Passthrough error from the target API |

Errors from the target API are passed through with their original status codes and response bodies.

### Troubleshooting: No Connection

If you get a 400/404, create a connection first:

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/connect/start" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider": "google", "service": "gmail"}'
# Open connect_url in browser to authorize
```

### Troubleshooting: Token Refresh Failed

A 500 error may indicate an expired OAuth token. Delete the connection and create a new one:

```bash
curl -s -X DELETE "$INTEGRACLAW_URL/api/v1/connections/{id}" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"

curl -s -X POST "$INTEGRACLAW_URL/api/v1/connect/start" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider": "google", "service": "gmail"}'
# Open connect_url in browser to re-authorize
```

## WebSocket (Real-time Connection Status)

```
ws://{host}/ws/connect/{session_id}?token=INTEGRACLAW_API_KEY
```

## Notes

1. **Use native API docs**: Refer to each service's official API documentation for endpoint paths and parameters.

2. **Headers are forwarded**: Custom headers (except `Host` and `Authorization`) are forwarded to the target API.

3. **Query params work**: URL query parameters are passed through to the target API.

4. **All HTTP methods supported**: GET, POST, PUT, PATCH, DELETE are all supported.

5. **Notion requires version header**: Always include `Notion-Version: 2022-06-28` for Notion API calls.
