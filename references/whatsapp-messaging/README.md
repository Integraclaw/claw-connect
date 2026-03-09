# WhatsApp Messaging Action Reference

**Provider:** `whatsapp`
**Service:** `messaging`
**App name:** `whatsapp-messaging`

## Actions

### send_message

Send a text message to a WhatsApp number.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "whatsapp",
    "service": "messaging",
    "action": "send_message",
    "params": {
      "phone_number_id": "PHONE_NUMBER_ID",
      "to": "5511999999999",
      "text": "Hello from IntegraClaw!"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `phone_number_id` | string | yes | WhatsApp Business phone number ID |
| `to` | string | yes | Recipient number (E.164 format, no +) |
| `text` | string | yes | Message body |

### send_template

Send a pre-approved template message.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "whatsapp",
    "service": "messaging",
    "action": "send_template",
    "params": {
      "phone_number_id": "PHONE_NUMBER_ID",
      "to": "5511999999999",
      "template_name": "hello_world",
      "template_language": "en_US",
      "components": []
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `phone_number_id` | string | yes | WhatsApp Business phone number ID |
| `to` | string | yes | Recipient number (E.164 format) |
| `template_name` | string | yes | Template name |
| `template_language` | string | yes | Template language code (e.g. en_US) |
| `components` | array | no | Template components with dynamic values |

### get_media

Get media file metadata and URL.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "whatsapp",
    "service": "messaging",
    "action": "get_media",
    "params": {"media_id": "MEDIA_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `media_id` | string | yes | Media ID from incoming message |

## Resources

- [WhatsApp Business API Reference](https://developers.facebook.com/docs/whatsapp/cloud-api/reference/messages)
