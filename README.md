# Claw Connect

Managed OAuth connector for AI agents. Semantic actions and direct token access to Google Workspace, Microsoft 365, and Notion — all through a single API key.

## What It Does

Your AI agent calls IntegraClaw's action API. IntegraClaw handles OAuth tokens, refresh, and API quirks. Your agent gets clean JSON back.

```
Agent → IntegraClaw → Google/Microsoft/Notion → IntegraClaw → Agent
```

Two modes:

- **Semantic Actions** — `POST /api/v1/action` with structured params. Zero token management.
- **Direct Tokens** — `POST /api/v1/token/get` for a fresh OAuth token. Call native APIs yourself.

## Quick Start

```bash
export INTEGRACLAW_API_KEY="ic_YOUR_KEY"

# Send an email
curl -s -X POST 'http://localhost:8443/api/v1/action' \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{"provider":"google","service":"gmail","action":"send_email","params":{"to":"user@example.com","subject":"Hello","body":"Sent via Claw Connect"}}'
```

## Supported Services

| Google | Microsoft | Notion |
|--------|-----------|--------|
| Gmail | Outlook | Pages |
| Calendar | Calendar | Databases |
| Drive | Teams | Blocks |
| Sheets | OneDrive | |

## Documentation

See [SKILL.md](SKILL.md) for full documentation, cookbook examples, and API reference.

## License

MIT
