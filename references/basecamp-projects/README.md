# Basecamp Projects Action Reference

**Provider:** `basecamp`
**Service:** `projects`
**App name:** `basecamp-projects`
**Auth:** API Key

## Actions

### list_projects

List projects in a Basecamp account.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "basecamp",
    "service": "projects",
    "action": "list_projects",
    "params": {"account_id": "1234567"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `account_id` | string | yes | Basecamp account ID |

### get_project

Get a project by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "basecamp",
    "service": "projects",
    "action": "get_project",
    "params": {
      "account_id": "1234567",
      "project_id": "9876543"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `account_id` | string | yes | Basecamp account ID |
| `project_id` | string | yes | Project ID |

### list_todolists

List todo lists in a project.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "basecamp",
    "service": "projects",
    "action": "list_todolists",
    "params": {
      "account_id": "1234567",
      "project_id": "9876543"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `account_id` | string | yes | Basecamp account ID |
| `project_id` | string | yes | Project ID |

### create_todo

Create a todo item in a list.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "basecamp",
    "service": "projects",
    "action": "create_todo",
    "params": {
      "account_id": "1234567",
      "project_id": "9876543",
      "todolist_id": "4567890",
      "content": "Follow up with client on proposal"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `account_id` | string | yes | Basecamp account ID |
| `project_id` | string | yes | Project ID |
| `todolist_id` | string | yes | Todo list ID |
| `content` | string | yes | Todo item content |
