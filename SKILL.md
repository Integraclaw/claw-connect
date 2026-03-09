---
name: claw-connect
description: |
  Connect to 40+ APIs (Google Workspace, Microsoft 365, Slack, HubSpot, Salesforce, Jira, Stripe, Notion, etc.) with managed OAuth.
  Use this skill when users want to interact with external services.
  Security: The INTEGRACLAW_API_KEY authenticates with IntegraClaw but grants NO access to third-party services by itself. Each service requires explicit OAuth authorization by the user through IntegraClaw's dashboard. Access is strictly scoped to connections the user has authorized. Provided by IntegraClaw (https://integraclaw.dev).
compatibility: Requires network access and valid IntegraClaw API key
metadata:
  author: integraclaw
  version: "2.0"
  homepage: "https://integraclaw.dev"
  requires:
    env:
      - INTEGRACLAW_URL
      - INTEGRACLAW_API_KEY
---

# Claw Connect

Semantic actions for third-party APIs using managed OAuth connections, provided by [IntegraClaw](https://integraclaw.dev). IntegraClaw lets you call service actions through a single unified API.

IMPORTANT: Connections are managed by users through the IntegraClaw dashboard. The agent's role is to **list available connections** and **call actions** using the references for each service. The agent never manages OAuth flows, tokens, or connection setup.

## Quick Start

```bash
# List available connections
curl -s "$INTEGRACLAW_URL/api/v1/connections" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"

# Call an action (see references for params)
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"gmail","action":"send_email","params":{"to":"user@example.com","subject":"Hello!","body":"Sent via IntegraClaw"}}'
```

## Base URL

```
$INTEGRACLAW_URL/api/v1
```

## Authentication

All requests require the IntegraClaw API key in the Authorization header:

```
Authorization: Bearer $INTEGRACLAW_API_KEY
```

**Environment variables:**

```bash
export INTEGRACLAW_URL="https://your-instance.com"
export INTEGRACLAW_API_KEY="ic_YOUR_KEY"
```

## How It Works

1. **User connects services** through the IntegraClaw dashboard (OAuth or API key)
2. **Agent lists connections** via `GET /api/v1/connections` to see what's available
3. **Agent calls actions** via `POST /api/v1/action` — IntegraClaw handles token management automatically
4. **Agent consults references** to know which actions exist and what params they accept

The agent never manages OAuth flows, tokens, or connection setup. That's all handled by IntegraClaw.

## List Connections

Check which services the user has connected:

```bash
curl -s "$INTEGRACLAW_URL/api/v1/connections" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

**Response:**
```json
[
  {
    "id": "21fd90f9-...",
    "provider": "google",
    "service": "gmail",
    "email": "user@gmail.com",
    "status": "connected"
  }
]
```

Each connection has `provider`, `service`, `status`, and optionally `email`. Only use connections with `status: "connected"`.

You can filter by app:

```bash
curl -s "$INTEGRACLAW_URL/api/v1/connections?app=google-gmail&status=connected" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

## Action API

Execute actions through `POST /api/v1/action`:

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"PROVIDER","service":"SERVICE","action":"ACTION_NAME","params":{...}}'
```

**Request Body:**
- `provider` (required) — Provider name (see table below)
- `service` (required) — Service name (see table below)
- `action` (required) — Action name (see references for each service)
- `params` (required) — Action parameters (see references for each action)
- `connection_id` (optional) — Specific connection ID. If omitted, uses the first active connection for that provider/service.

**Response:**
```json
{
  "success": true,
  "status": 200,
  "data": { ... }
}
```

**Error Response:**
```json
{
  "success": false,
  "status": 400,
  "error": "missing required parameter: to"
}
```

## Discover Actions

List all available actions:

```bash
curl -s "$INTEGRACLAW_URL/api/v1/actions" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

Filter by app:

```bash
curl -s "$INTEGRACLAW_URL/api/v1/actions?app=google-gmail" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

List actions with full parameter schemas (MCP-compatible tools):

```bash
curl -s "$INTEGRACLAW_URL/api/v1/tools" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

## Supported Services

### Google Workspace

| Service | App Name | Actions |
|---------|----------|---------|
| Gmail | `google-gmail` | send_email, list_messages, search_messages, read_message, mark_as_read, archive, list_labels, create_draft |
| Calendar | `google-calendar` | list_calendars, list_events, search_events, get_event, create_event, update_event, delete_event, quick_add |
| Drive | `google-drive` | list_files, search_files, get_file, create_folder, copy_file, update_file, delete_file |
| Sheets | `google-sheets` | get_spreadsheet, read_range, write_range, append_rows, clear_range, create_spreadsheet |
| Docs | `google-docs` | create_document, get_document, insert_text, replace_text |
| Slides | `google-slides` | get_presentation, create_presentation, get_page, batch_update |
| Forms | `google-forms` | get_form, create_form, list_responses, get_response |
| Tasks | `google-tasks` | list_task_lists, create_task_list, delete_task_list, list_tasks, get_task, create_task, update_task, complete_task |
| Contacts | `google-contacts` | list_contacts, search_contacts, get_contact, create_contact, delete_contact |
| Meet | `google-meet` | create_space, get_space, end_conference, list_conference_records |
| YouTube | `google-youtube` | list_channels, search_videos, get_video, list_playlists, list_playlist_items |
| Search Console | `google-search-console` | list_sites, get_site, query_analytics, list_sitemaps |
| Analytics Admin | `google-analytics-admin` | list_accounts, list_properties, get_property, list_data_streams |
| Analytics Data | `google-analytics-data` | run_report, run_realtime_report, get_metadata |
| Ads | `google-ads` | search, list_customers, get_customer |
| Play | `google-play` | list_reviews, get_review, reply_review |
| Workspace Admin | `google-workspace-admin` | list_users, get_user, list_groups, get_group, list_group_members |

### Microsoft 365

| Service | App Name | Actions |
|---------|----------|---------|
| Outlook | `microsoft-outlook` | send_email, list_messages, search_messages, read_message, list_folders |
| Calendar | `microsoft-calendar` | list_events, get_event, create_event, delete_event |
| Teams | `microsoft-teams` | list_teams, list_channels, send_message, list_messages |
| OneDrive | `microsoft-onedrive` | list_files, search_files, get_file, create_folder |

### Notion

| Service | App Name | Actions |
|---------|----------|---------|
| Pages | `notion-pages` | search, get, create, update |
| Databases | `notion-databases` | query, get, create |
| Blocks | `notion-blocks` | get_children, append, delete |

### CRM & Sales

| Provider | Service | App Name | Actions |
|----------|---------|----------|---------|
| HubSpot | CRM | `hubspot-crm` | list_contacts, get_contact, create_contact, search_contacts, list_deals, create_deal |
| Salesforce | CRM | `salesforce-crm` | query, get_record, create_record, update_record |
| Pipedrive | Deals | `pipedrive-deals` | list_deals, get_deal, create_deal, update_deal, search_deals |
| Attio | CRM | `attio-crm` | list_objects, list_records, get_record |
| ActiveCampaign | Contacts | `activecampaign-contacts` | list_contacts, get_contact, create_contact, search_contacts |
| Kommo | Leads | `kommo-leads` | list, get, create, update |
| Kommo | Contacts | `kommo-contacts` | list, get, create, update |
| Kommo | Pipelines | `kommo-pipelines` | list, get, statuses |

### Project Management & Tasks

| Provider | Service | App Name | Actions |
|----------|---------|----------|---------|
| Jira | Issues | `jira-issues` | search, get_issue, create_issue, list_projects |
| Asana | Tasks | `asana-tasks` | list_workspaces, list_projects, list_tasks, get_task, create_task, update_task |
| Trello | Boards | `trello-boards` | list_boards, get_board, list_lists, list_cards, create_card |
| Monday | Boards | `monday-boards` | list_boards, get_board, list_items, create_item |
| ClickUp | Tasks | `clickup-tasks` | list, get, create, update, delete |
| ClickUp | Spaces | `clickup-spaces` | list, get_lists, get_folders |
| ClickUp | Workspaces | `clickup-workspaces` | list |
| Basecamp | Projects | `basecamp-projects` | list_projects, get_project, list_todolists, create_todo |

### Messaging & Communication

| Provider | Service | App Name | Actions |
|----------|---------|----------|---------|
| Slack | Messaging | `slack-messaging` | send_message, list_channels, list_messages, search_messages |
| WhatsApp | Messaging | `whatsapp-messaging` | send_message, send_template, get_media |
| ClickSend | SMS | `clicksend-sms` | send_sms, list_sms_history |

### Scheduling

| Provider | Service | App Name | Actions |
|----------|---------|----------|---------|
| Calendly | Scheduling | `calendly-scheduling` | get_current_user, list_event_types, list_scheduled_events, get_scheduled_event, list_invitees |
| Acuity | Scheduling | `acuity-scheduling` | list_appointments, list_appointment_types |
| Cal.com | Scheduling | `calcom-scheduling` | list_event_types, list_bookings |

### Finance & Payments

| Provider | Service | App Name | Actions |
|----------|---------|----------|---------|
| Stripe | Payments | `stripe-payments` | list_charges, list_customers, get_customer, list_subscriptions |
| QuickBooks | Accounting | `quickbooks-accounting` | query, get_customer, get_invoice, create_invoice |

| Chargebee | Subscriptions | `chargebee-subscriptions` | list_subscriptions, list_customers |
| ContaAzul | Customers | `contaazul-customers` | list, get, create, update |
| ContaAzul | Products | `contaazul-products` | list, get, create, update |
| ContaAzul | Sales | `contaazul-sales` | list, get, create |

### Forms & Data

| Provider | Service | App Name | Actions |
|----------|---------|----------|---------|
| Typeform | Forms | `typeform-forms` | list_forms, get_form, list_responses |
| Airtable | Bases | `airtable-bases` | list_bases, list_records, get_record, create_records, update_records |
| Baserow | Databases | `baserow-databases` | list_tables, list_rows |

### Storage

| Provider | Service | App Name | Actions |
|----------|---------|----------|---------|
| Box | Files | `box-files` | list_folder_items, get_file, search, create_folder |

### Marketing & Outreach

| Provider | Service | App Name | Actions |
|----------|---------|----------|---------|
| Apollo | Prospecting | `apollo-prospecting` | search_people, get_person, search_organizations |
| Klaviyo | Campaigns | `klaviyo-campaigns` | list_campaigns, get_campaign, list_lists |
| Brevo | Email | `brevo-email` | send_email, list_contacts, create_contact |
| Beehiiv | Newsletters | `beehiiv-newsletters` | list_publications, list_subscribers |
| ClickFunnels | Funnels | `clickfunnels-funnels` | list_workspaces, list_contacts |

### Analytics

| Provider | Service | App Name | Actions |
|----------|---------|----------|---------|
| CallRail | Calls | `callrail-calls` | list_calls |

See [references/](references/) for detailed action guides per service:

**Google Workspace:**
- [Gmail](references/google-mail/README.md) — Send, list, search, read, draft
- [Google Calendar](references/google-calendar/README.md) — Events, calendars, quick add
- [Google Drive](references/google-drive/README.md) — Files, folders, search
- [Google Sheets](references/google-sheets/README.md) — Read, write, append, create
- [Google Docs](references/google-docs/README.md) — Create, read, insert text, find & replace
- [Google Slides](references/google-slides/README.md) — Presentations, pages, batch update
- [Google Forms](references/google-forms/README.md) — Forms, responses
- [Google Tasks](references/google-tasks/README.md) — Task lists, create, complete, subtasks
- [Google Contacts](references/google-contacts/README.md) — List, search, create, delete contacts
- [Google Meet](references/google-meet/README.md) — Create spaces, end conferences, records
- [Google YouTube](references/google-youtube/README.md) — Channels, videos, playlists
- [Google Search Console](references/google-search-console/README.md) — Sites, analytics, sitemaps
- [Google Analytics Admin](references/google-analytics-admin/README.md) — Accounts, properties, data streams
- [Google Analytics Data](references/google-analytics-data/README.md) — Reports, realtime, metadata
- [Google Ads](references/google-ads/README.md) — Search, customers
- [Google Play](references/google-play/README.md) — Reviews, replies
- [Google Workspace Admin](references/google-workspace-admin/README.md) — Users, groups, members

**Microsoft 365:**
- [Outlook](references/outlook/README.md) — Send, list, search, read, folders
- [Microsoft Calendar](references/microsoft-calendar/README.md) — Events, create, delete
- [Microsoft Teams](references/microsoft-teams/README.md) — Teams, channels, messages
- [OneDrive](references/one-drive/README.md) — Files, folders, search

**Third-party (OAuth):**
- [Notion](references/notion/README.md) — Pages, databases, blocks
- [Slack](references/slack-messaging/README.md) — Send messages, channels, search
- [HubSpot](references/hubspot-crm/README.md) — Contacts, deals, search
- [Salesforce](references/salesforce-crm/README.md) — SOQL query, records CRUD
- [Jira](references/jira-issues/README.md) — Issues, search, projects
- [Calendly](references/calendly-scheduling/README.md) — Events, invitees
- [Asana](references/asana-tasks/README.md) — Workspaces, projects, tasks
- [Monday](references/monday-boards/README.md) — Boards, items
- [Pipedrive](references/pipedrive-deals/README.md) — Deals, search
- [Typeform](references/typeform-forms/README.md) — Forms, responses
- [Airtable](references/airtable-bases/README.md) — Bases, records
- [QuickBooks](references/quickbooks-accounting/README.md) — Query, invoices, customers

- [Box](references/box-files/README.md) — Files, folders, search
- [Stripe](references/stripe-payments/README.md) — Charges, customers, subscriptions
- [Trello](references/trello-boards/README.md) — Boards, lists, cards
- [WhatsApp](references/whatsapp-messaging/README.md) — Messages, templates
- [ClickUp](references/clickup-tasks/README.md) — Tasks, spaces, workspaces
- [ContaAzul](references/contaazul/README.md) — Customers, products, sales
- [Kommo](references/kommo/README.md) — Leads, contacts, pipelines

**Third-party (API Key):**
- [ActiveCampaign](references/activecampaign-contacts/README.md) — Contacts, search
- [Klaviyo](references/klaviyo-campaigns/README.md) — Campaigns, lists
- [Apollo](references/apollo-prospecting/README.md) — People, organizations
- [Brevo](references/brevo-email/README.md) — Transactional email, contacts
- [Beehiiv](references/beehiiv-newsletters/README.md) — Publications, subscribers
- [Baserow](references/baserow-databases/README.md) — Tables, rows
- [Cal.com](references/calcom-scheduling/README.md) — Event types, bookings
- [CallRail](references/callrail-calls/README.md) — Call tracking
- [Chargebee](references/chargebee-subscriptions/README.md) — Subscriptions, customers
- [ClickSend](references/clicksend-sms/README.md) — SMS, history
- [ClickFunnels](references/clickfunnels-funnels/README.md) — Workspaces, contacts
- [Attio](references/attio-crm/README.md) — Objects, records
- [Acuity](references/acuity-scheduling/README.md) — Appointments
- [Basecamp](references/basecamp-projects/README.md) — Projects, todolists

## Examples

### Gmail — Send Email

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"gmail","action":"send_email","params":{"to":"user@example.com","subject":"Weekly Report","body":"Please find the report attached.","cc":"manager@example.com"}}'
```

### Gmail — Search Messages

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"gmail","action":"search_messages","params":{"query":"from:boss@company.com newer_than:7d","max_results":5}}'
```

### Google Calendar — Create Event

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"calendar","action":"create_event","params":{"summary":"Team Meeting","start":"2026-03-10T10:00:00","end":"2026-03-10T11:00:00","timezone":"America/Sao_Paulo","attendees":["colleague@example.com"]}}'
```

### Google Sheets — Read Values

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"sheets","action":"read_range","params":{"spreadsheet_id":"SPREADSHEET_ID","range":"Sheet1!A1:D10"}}'
```

### Google Sheets — Append Rows

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"google","service":"sheets","action":"append_rows","params":{"spreadsheet_id":"SPREADSHEET_ID","range":"Sheet1!A1","values":[["Name","Email","Status"],["John","john@example.com","Active"]]}}'
```

### Notion — Query Database

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"notion","service":"databases","action":"query","params":{"database_id":"DATABASE_ID","filter":{"property":"Status","select":{"equals":"Active"}},"page_size":50}}'
```

### Slack — Send Message

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"slack","service":"messaging","action":"send_message","params":{"channel":"C0123456789","text":"Hello from IntegraClaw!"}}'
```

### HubSpot — Create Contact

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"hubspot","service":"crm","action":"create_contact","params":{"email":"john@example.com","firstname":"John","lastname":"Doe"}}'
```

### Salesforce — SOQL Query

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"salesforce","service":"crm","action":"query","params":{"soql":"SELECT Id, Name, Email FROM Contact WHERE CreatedDate = LAST_N_DAYS:30 LIMIT 10"}}'
```

### Jira — Create Issue

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"jira","service":"issues","action":"create_issue","params":{"project_key":"PROJ","summary":"Fix login bug","issue_type":"Bug","description":"Users cannot log in with SSO"}}'
```

### Stripe — List Customers

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"stripe","service":"payments","action":"list_customers","params":{"limit":10,"email":"john@example.com"}}'
```

### Airtable — List Records

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"airtable","service":"bases","action":"list_records","params":{"base_id":"appXXXXXXX","table_name":"Tasks","max_records":20}}'
```

### WhatsApp — Send Message

```bash
curl -s -X POST "$INTEGRACLAW_URL/api/v1/action" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"provider":"whatsapp","service":"messaging","action":"send_message","params":{"phone_number_id":"PHONE_NUMBER_ID","to":"5511999999999","text":"Hello from IntegraClaw!"}}'
```

## Error Handling

| Status | Meaning | What to do |
|--------|---------|------------|
| 400 | Invalid request, missing params, or unknown action | Check the reference for correct action name and required params |
| 401 | Invalid or missing API key | Verify `INTEGRACLAW_API_KEY` is set correctly |
| 404 | No active connection for provider/service | Ask the user to connect the service through the IntegraClaw dashboard |
| 4xx/5xx | Passthrough error from the target API | Check the `error` field for details from the third-party API |

### Troubleshooting

1. **Check env vars are set:**

```bash
echo "URL: $INTEGRACLAW_URL"
echo "KEY: ${INTEGRACLAW_API_KEY:0:10}..."
```

2. **Verify API key by listing connections:**

```bash
curl -s "$INTEGRACLAW_URL/api/v1/connections" \
  -H "Authorization: Bearer $INTEGRACLAW_API_KEY"
```

3. **No connection for a service?** Ask the user to connect it through the IntegraClaw dashboard. The agent cannot create connections.

## Tips

1. **Always list connections first** — before calling any action, check what services the user has connected.

2. **Use `connection_id` for disambiguation** — if the user has multiple connections for the same provider/service, specify which one to use.

3. **Consult references for params** — each service has a detailed reference in [references/](references/) with all actions and their parameters.

4. **Use `/api/v1/actions?app=APP_NAME`** to discover available actions for a specific service.

5. **Use `/api/v1/tools`** to get MCP-compatible tool schemas with full parameter definitions.

6. **Response data is raw** — the `data` field in the response contains the raw API response from the third-party service. Refer to the service's native API docs for response format details.

7. **One API for everything** — every action follows the same pattern: `POST /api/v1/action` with `provider`, `service`, `action`, `params`. No need to know different API endpoints.

## Notes

1. The `action` field in the request maps to the tool method as `{provider}_{service}_{action}`.
2. IntegraClaw handles all OAuth token refresh automatically — the agent never deals with tokens.
3. Use `connection_id` only when the user has multiple connections for the same service.
4. If a 404 error occurs, the user needs to connect the service through the dashboard first. The agent cannot create connections.
5. All native API docs are still relevant for understanding response formats — IntegraClaw returns the raw API response in the `data` field.
