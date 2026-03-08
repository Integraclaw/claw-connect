# Claw Connect

API gateway for calling third-party APIs with managed auth.

Call native API endpoints directly with a single API key.

## Quick Start

```bash
# List Gmail messages
curl -s "$INTEGRACLAW_URL/gateway/google-mail/gmail/v1/users/me/messages?maxResults=5" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

## Getting Your API Key

1. Sign in or create an account at your IntegraClaw instance
2. Go to Settings → API Keys
3. Create a new key and copy it

```bash
export INTEGRACLAW_API_KEY="ic_YOUR_KEY"
export INTEGRACLAW_URL="http://localhost:8443"
```

## License

MIT
