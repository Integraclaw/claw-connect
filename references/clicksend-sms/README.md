# ClickSend SMS Action Reference

**Provider:** `clicksend`
**Service:** `sms`
**App name:** `clicksend-sms`
**Auth:** API Key

## Actions

### send_sms

Send an SMS message.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "clicksend",
    "service": "sms",
    "action": "send_sms",
    "params": {
      "to": "+15551234567",
      "body": "Your verification code is 123456",
      "from": "Acme"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `to` | string | yes | Recipient phone number (E.164 format) |
| `body` | string | yes | SMS message content |
| `from` | string | no | Sender ID or phone number |

### list_sms_history

List sent and received SMS messages.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "clicksend",
    "service": "sms",
    "action": "list_sms_history",
    "params": {"page": 1, "limit": 50}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `page` | integer | no | Page number for pagination |
| `limit` | integer | no | Messages per page |
