# Google Tasks Action Reference

**Provider:** `google`
**Service:** `tasks`
**App name:** `google-tasks`

## Actions

### list_task_lists

List all task lists for the authenticated user.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "tasks",
    "action": "list_task_lists",
    "params": {}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `max_results` | integer | no | Maximum task lists to return (default 100, max 100) |
| `page_token` | string | no | Pagination token |

### create_task_list

Create a new task list.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "tasks",
    "action": "create_task_list",
    "params": {
      "title": "My Project Tasks"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `title` | string | **yes** | Title of the task list (max 1024 chars) |

### delete_task_list

Delete a task list and all its tasks.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "tasks",
    "action": "delete_task_list",
    "params": {
      "tasklist_id": "TASKLIST_ID"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tasklist_id` | string | **yes** | Task list ID |

### list_tasks

List tasks from a task list.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "tasks",
    "action": "list_tasks",
    "params": {
      "tasklist_id": "@default",
      "max_results": 20
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tasklist_id` | string | no | Task list ID (default: `@default` for primary list) |
| `max_results` | integer | no | Maximum tasks to return (default 20, max 100) |
| `page_token` | string | no | Pagination token |
| `show_completed` | boolean | no | Show completed tasks (default true) |
| `due_min` | string | no | Lower bound for due date (RFC3339) |
| `due_max` | string | no | Upper bound for due date (RFC3339) |

### get_task

Get full details of a specific task.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "tasks",
    "action": "get_task",
    "params": {
      "tasklist_id": "TASKLIST_ID",
      "task_id": "TASK_ID"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tasklist_id` | string | **yes** | Task list ID |
| `task_id` | string | **yes** | Task ID |

### create_task

Create a new task in a task list.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "tasks",
    "action": "create_task",
    "params": {
      "title": "Review pull request",
      "notes": "Check for security issues",
      "due": "2026-03-15T00:00:00Z"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `title` | string | **yes** | Task title (max 1024 chars) |
| `tasklist_id` | string | no | Task list ID (default: `@default`) |
| `notes` | string | no | Task notes/description (max 8192 chars) |
| `due` | string | no | Due date (RFC3339) |
| `parent_task_id` | string | no | Parent task ID (to create a subtask) |

### update_task

Update an existing task.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "tasks",
    "action": "update_task",
    "params": {
      "tasklist_id": "TASKLIST_ID",
      "task_id": "TASK_ID",
      "title": "Updated title",
      "due": "2026-03-20T00:00:00Z"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tasklist_id` | string | **yes** | Task list ID |
| `task_id` | string | **yes** | Task ID |
| `title` | string | no | New task title |
| `notes` | string | no | New task notes |
| `due` | string | no | New due date (RFC3339) |
| `status` | string | no | Task status: `needsAction` or `completed` |

### complete_task

Mark a task as completed.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "tasks",
    "action": "complete_task",
    "params": {
      "tasklist_id": "TASKLIST_ID",
      "task_id": "TASK_ID"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tasklist_id` | string | **yes** | Task list ID |
| `task_id` | string | **yes** | Task ID |
