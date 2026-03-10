# Claw Connect

API gateway with managed auth. Connect your AI to Google Workspace (Gmail, Calendar, Drive, Sheets, Docs, Meet, YouTube, and more) with a single API key.

## Quick Start

```bash
# List available connections
curl -s "$INTEGRACLAW_URL/api/v1/connections" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"

# Call an action (see references/ for params)
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"gmail","action":"send_email","params":{"to":"user@example.com","subject":"Hello!","body":"Sent via Integraclaw"}}'
```

## How It Works

1. **Users connect services** through the Integraclaw dashboard (OAuth)
2. **Agent lists connections** — `GET /api/v1/connections`
3. **Agent calls actions** — `POST /api/v1/action` (see [references/](references/) for each service)

The agent never manages OAuth, tokens, or connection setup. Integraclaw handles all token management automatically.

## Supported Services

Google Workspace: Gmail, Calendar, Drive, Sheets, Docs, Slides, Forms, Tasks, Contacts, Meet, YouTube, Search Console, Analytics Admin, Analytics Data, Ads, Play, Workspace Admin.

More providers coming soon.

See [SKILL.md](SKILL.md) for the full table with app names and actions, and [references/](references/) for detailed action guides per service.

## License

MIT
