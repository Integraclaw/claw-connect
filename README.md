# Claw Connect

API gateway with managed auth. Connect your AI to Google Workspace, Microsoft 365, Slack, HubSpot, Salesforce, Jira, Stripe, Notion, and more with a single API key.

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

1. **Users connect services** through the Integraclaw dashboard (OAuth or API key)
2. **Agent lists connections** — `GET /api/v1/connections`
3. **Agent calls actions** — `POST /api/v1/action` (see [references/](references/) for each service)

The agent never manages OAuth, tokens, or connection setup. Integraclaw handles all token management automatically.

## Supported Services (40+)

Google Workspace (17), Microsoft 365 (4), Notion (3), Slack, HubSpot, Salesforce, Jira, Calendly, Asana, Monday, Pipedrive, Typeform, Airtable, QuickBooks, Box, Stripe, Trello, WhatsApp, ClickUp, ContaAzul, Kommo, ActiveCampaign, Klaviyo, Apollo, Brevo, Beehiiv, Baserow, Cal.com, CallRail, Chargebee, ClickSend, ClickFunnels, Attio, Acuity, Basecamp.

See [SKILL.md](SKILL.md) for the full table with app names and actions, and [references/](references/) for detailed action guides per service.

## License

MIT
