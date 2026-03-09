# Asana Tasks Action Reference

**Provider:** `asana`
**Service:** `tasks`
**App name:** `asana-tasks`

## Actions

### list_workspaces

List workspaces (organizations).

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "asana",
    "service": "tasks",
    "action": "list_workspaces",
    "params": {}
  }'
```

No parameters required.

### list_projects

List projects in a workspace.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "asana",
    "service": "tasks",
    "action": "list_projects",
    "params": {"workspace": "123456789"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `workspace` | string | yes | Workspace GID |

### list_tasks

List tasks in a project.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "asana",
    "service": "tasks",
    "action": "list_tasks",
    "params": {"project": "987654321"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | yes | Project GID |

### get_task

Get a task by GID.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "asana",
    "service": "tasks",
    "action": "get_task",
    "params": {"task_gid": "1234567890123456"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `task_gid` | string | yes | Task GID |

### create_task

Create a new task.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "asana",
    "service": "tasks",
    "action": "create_task",
    "params": {
      "workspace": "123456789",
      "name": "Review design mockups",
      "projects": ["987654321"],
      "notes": "Check color palette and typography.",
      "due_on": "2026-03-15",
      "assignee": "user_gid"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `workspace` | string | yes | Workspace GID |
| `name` | string | yes | Task name |
| `projects` | array | no | Project GIDs to add task to |
| `notes` | string | no | Task description |
| `due_on` | string | no | Due date (YYYY-MM-DD) |
| `assignee` | string | no | Assignee user GID |

### update_task

Update a task.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "asana",
    "service": "tasks",
    "action": "update_task",
    "params": {
      "task_gid": "1234567890123456",
      "name": "Updated task name",
      "completed": true,
      "due_on": "2026-03-20"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `task_gid` | string | yes | Task GID |
| `name` | string | no | New task name |
| `completed` | boolean | no | Mark as completed |
| `due_on` | string | no | New due date (YYYY-MM-DD) |

## Resources

- [Asana API Reference](https://developers.asana.com/reference/rest-api-reference)
