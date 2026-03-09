# ClickUp Action Reference

**Provider:** `clickup`
**Services:** `tasks`, `spaces`, `workspaces`

ClickUp actions are split across three services:

| Service | App Name | Actions |
|---------|----------|---------|
| Tasks | `clickup-tasks` | list, get, create, update, delete |
| Spaces | `clickup-spaces` | list, get_lists, get_folders |
| Workspaces | `clickup-workspaces` | list |

---

## Tasks

### list

List tasks in a list.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "clickup",
    "service": "tasks",
    "action": "list",
    "params": {"list_id": "LIST_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `list_id` | string | yes | List ID |

### get

Get a task by ID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "clickup",
    "service": "tasks",
    "action": "get",
    "params": {"task_id": "TASK_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `task_id` | string | yes | Task ID |

### create

Create a new task.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "clickup",
    "service": "tasks",
    "action": "create",
    "params": {
      "list_id": "LIST_ID",
      "name": "New task",
      "description": "Task details",
      "priority": 3
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `list_id` | string | yes | List ID |
| `name` | string | yes | Task name |
| `description` | string | no | Task description |
| `priority` | integer | no | Priority (1=urgent, 2=high, 3=normal, 4=low) |

### update

Update a task.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "clickup",
    "service": "tasks",
    "action": "update",
    "params": {
      "task_id": "TASK_ID",
      "name": "Updated name",
      "status": "complete"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `task_id` | string | yes | Task ID |
| `name` | string | no | New task name |
| `status` | string | no | Status name |

### delete

Delete a task.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "clickup",
    "service": "tasks",
    "action": "delete",
    "params": {"task_id": "TASK_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `task_id` | string | yes | Task ID |

---

## Spaces

### list

List spaces in a team.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "clickup",
    "service": "spaces",
    "action": "list",
    "params": {"team_id": "TEAM_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `team_id` | string | yes | Team ID |

### get_lists

Get lists in a space.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "clickup",
    "service": "spaces",
    "action": "get_lists",
    "params": {"space_id": "SPACE_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `space_id` | string | yes | Space ID |

### get_folders

Get folders in a space.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "clickup",
    "service": "spaces",
    "action": "get_folders",
    "params": {"space_id": "SPACE_ID"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `space_id` | string | yes | Space ID |

---

## Workspaces

### list

List workspaces (teams).

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "clickup",
    "service": "workspaces",
    "action": "list",
    "params": {}
  }'
```

No parameters required.

---

## Resources

- [ClickUp API Reference](https://clickup.com/api)
