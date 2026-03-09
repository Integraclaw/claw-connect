# Jira Issues Action Reference

**Provider:** `jira`
**Service:** `issues`
**App name:** `jira-issues`

## Actions

### search

Search issues using JQL.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "jira",
    "service": "issues",
    "action": "search",
    "params": {
      "jql": "project = MYPROJ AND status = Open",
      "max_results": 50,
      "start_at": 0
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `jql` | string | yes | JQL search query |
| `max_results` | integer | no | Maximum issues to return |
| `start_at` | integer | no | Pagination start index |

### get_issue

Get an issue by key.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "jira",
    "service": "issues",
    "action": "get_issue",
    "params": {"issue_key": "MYPROJ-123"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `issue_key` | string | yes | Issue key (e.g. MYPROJ-123) |

### create_issue

Create a new issue.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "jira",
    "service": "issues",
    "action": "create_issue",
    "params": {
      "project_key": "MYPROJ",
      "summary": "Implement login flow",
      "issue_type": "Task",
      "description": "As a user, I want to log in with email and password."
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project_key` | string | yes | Project key |
| `summary` | string | yes | Issue summary/title |
| `issue_type` | string | yes | Issue type (Task, Bug, Story, etc.) |
| `description` | string | no | Issue description |

### list_projects

List accessible Jira projects.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "jira",
    "service": "issues",
    "action": "list_projects",
    "params": {"max_results": 50}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `max_results` | integer | no | Maximum projects to return |

## Resources

- [Jira REST API Reference](https://developer.atlassian.com/cloud/jira/platform/rest/v3/)
- [JQL Reference](https://support.atlassian.com/jira-service-management-cloud/docs/use-advanced-search-with-jira-query-language-jql/)
