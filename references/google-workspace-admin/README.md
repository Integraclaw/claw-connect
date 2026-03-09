# Google Workspace Admin Action Reference

**Provider:** `google`
**Service:** `workspace-admin`
**App name:** `google-workspace-admin`

## Actions

### list_users

List users in a Google Workspace domain.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "workspace-admin",
    "action": "list_users",
    "params": {
      "domain": "example.com",
      "max_results": 500,
      "page_token": "NEXT_PAGE_TOKEN"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `domain` | string | no | Filter by domain (required for multi-domain orgs) |
| `max_results` | integer | no | Max users per page |
| `page_token` | string | no | Pagination token from previous response |

### get_user

Retrieve details for a specific user.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "workspace-admin",
    "action": "get_user",
    "params": {"user_key": "alice@example.com"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `user_key` | string | yes | User email or numeric user ID |

### list_groups

List groups in a Google Workspace domain.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "workspace-admin",
    "action": "list_groups",
    "params": {
      "domain": "example.com",
      "max_results": 200,
      "page_token": "NEXT_PAGE_TOKEN"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `domain` | string | no | Filter by domain |
| `max_results` | integer | no | Max groups per page |
| `page_token` | string | no | Pagination token from previous response |

### get_group

Retrieve details for a specific group.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "workspace-admin",
    "action": "get_group",
    "params": {"group_key": "engineering@example.com"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `group_key` | string | yes | Group email or numeric group ID |

### list_group_members

List members of a group.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "workspace-admin",
    "action": "list_group_members",
    "params": {
      "group_key": "engineering@example.com",
      "max_results": 100,
      "page_token": "NEXT_PAGE_TOKEN"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `group_key` | string | yes | Group email or numeric group ID |
| `max_results` | integer | no | Max members per page |
| `page_token` | string | no | Pagination token from previous response |
