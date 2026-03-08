# Claw Connect

Managed OAuth connector for AI agents. Call third-party APIs with a single API key.

IntegraClaw handles OAuth tokens, refresh, and API integration. Your agent sends structured actions and gets clean JSON back.

## Quick Start

```bash
export INTEGRACLAW_URL="http://localhost:8443"
export INTEGRACLAW_API_KEY="ic_YOUR_KEY"

# Send an email via Gmail
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

## Getting Your API Key

1. Sign in at your IntegraClaw instance
2. Go to Settings → API Keys
3. Create a new key and copy it

```bash
export INTEGRACLAW_API_KEY="ic_YOUR_KEY"
```

## License

MIT
