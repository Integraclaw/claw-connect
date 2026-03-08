# IntegraClaw API Gateway

API gateway for calling third-party APIs with managed OAuth.

Execute pre-built actions or get fresh OAuth tokens for direct native API calls with a single API key.

## Quick Start

```bash
# Send an email via Gmail
curl -s -X POST 'http://localhost:8443/api/v1/action' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_API_KEY' \
  -d '{"provider":"google","service":"gmail","action":"send_email","params":{"to":"user@example.com","subject":"Hello!","body":"Hello from IntegraClaw!"}}'
```

## Getting Your API Key

1. Sign in to IntegraClaw dashboard
2. Go to **API Keys** section
3. Click **Create API Key** and copy it

```bash
export INTEGRACLAW_API_KEY="ic_YOUR_API_KEY"
```

## Supported Services

| Provider | Services |
|----------|----------|
| Google | Gmail, Calendar, Drive, Sheets |
| Microsoft | Outlook, Calendar, Teams, OneDrive |
| Notion | Pages, Databases, Blocks |

## License

MIT
