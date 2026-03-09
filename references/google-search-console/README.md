# Google Search Console Action Reference

**Provider:** `google`
**Service:** `search-console`
**App name:** `google-search-console`

## Actions

### list_sites

List all verified sites in Search Console for the authenticated user.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "search-console",
    "action": "list_sites"
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| *none* | — | — | No parameters required |

### get_site

Retrieve details for a specific site.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "search-console",
    "action": "get_site",
    "params": {"site_url": "sc-domain:example.com"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `site_url` | string | yes | Site URL (e.g. `sc-domain:example.com` or `https://example.com/`) |

### query_analytics

Query Search Analytics data: clicks, impressions, CTR, and position by dimension.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "search-console",
    "action": "query_analytics",
    "params": {
      "site_url": "sc-domain:example.com",
      "start_date": "2024-01-01",
      "end_date": "2024-01-31",
      "dimensions": ["query", "page"],
      "row_limit": 1000
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `site_url` | string | yes | Site URL (e.g. `sc-domain:example.com`) |
| `start_date` | string | yes | Start date (YYYY-MM-DD) |
| `end_date` | string | yes | End date (YYYY-MM-DD) |
| `dimensions` | array | no | Dimensions to group by (query, page, country, device, etc.) |
| `row_limit` | integer | no | Max rows to return |

### list_sitemaps

List sitemaps submitted for a site.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "search-console",
    "action": "list_sitemaps",
    "params": {"site_url": "sc-domain:example.com"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `site_url` | string | yes | Site URL (e.g. `sc-domain:example.com`) |
