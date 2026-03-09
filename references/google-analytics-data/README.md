# Google Analytics Data Action Reference

**Provider:** `google`
**Service:** `analytics-data`
**App name:** `google-analytics-data`

## Actions

### run_report

Run a standard GA4 report with dimensions and metrics.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "analytics-data",
    "action": "run_report",
    "params": {
      "property_id": "123456789",
      "date_ranges": [{"start_date": "2024-01-01", "end_date": "2024-01-31"}],
      "dimensions": [{"name": "country"}, {"name": "pagePath"}],
      "metrics": [{"name": "sessions"}, {"name": "activeUsers"}]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `property_id` | string | yes | GA4 property ID |
| `date_ranges` | array | yes | Array of `{start_date, end_date}` objects (YYYY-MM-DD) |
| `dimensions` | array | no | Dimension names (e.g. `country`, `pagePath`, `date`) |
| `metrics` | array | yes | Metric names (e.g. `sessions`, `activeUsers`, `screenPageViews`) |

### run_realtime_report

Run a realtime report for live traffic data.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "analytics-data",
    "action": "run_realtime_report",
    "params": {
      "property_id": "123456789",
      "dimensions": [{"name": "country"}],
      "metrics": [{"name": "activeUsers"}]
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `property_id` | string | yes | GA4 property ID |
| `dimensions` | array | no | Dimension names for grouping |
| `metrics` | array | no | Metric names (e.g. `activeUsers`) |

### get_metadata

Retrieve available dimensions and metrics for a property.

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "analytics-data",
    "action": "get_metadata",
    "params": {"property_id": "123456789"}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `property_id` | string | yes | GA4 property ID |

## Common Dimensions & Metrics

- **Dimensions:** `country`, `date`, `pagePath`, `sessionSource`, `deviceCategory`
- **Metrics:** `sessions`, `activeUsers`, `screenPageViews`, `conversions`
