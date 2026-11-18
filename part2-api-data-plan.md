# Part 2 · API & Data Source Plan

## Goal
Power the AI Governance Dashboard with accurate, real-time (or near-real-time) data on
**who is using which AI tools, how much it costs, and whether spend/policy limits are respected.**

---

## Data We Need → Mapped to Sources

### 1. Spend & Usage per AI Tool

| Data Point | Primary Source | Method |
|---|---|---|
| OpenAI (ChatGPT) spend & token usage | **OpenAI Usage API** (`/v1/usage`) | REST poll (hourly) |
| Anthropic (Claude) spend | **Anthropic Usage API** | REST poll |
| Microsoft Copilot seats & activity | **Microsoft Graph API** (`/reports/getM365AppUserDetail`) | Webhook / daily |
| Midjourney usage | **Credit card / corporate card statements** | Plaid or Brex/Ramp API |
| Grammarly Business | **Grammarly Admin Dashboard API** | REST |
| Any tool billed to company card | **Expense management tools** (Brex, Ramp, Expensify) | Webhooks on new charges |

---

### 2. Identity & Team Attribution (Who is using what)

| Data Point | Source | Method |
|---|---|---|
| Employee → Department mapping | **HR system / HRIS** (Workday, BambooHR, Rippling) | REST API or SCIM sync |
| SSO login events | **Identity Provider** (Okta, Azure AD / Entra ID) | Okta System Log API or Azure AD Sign-In Logs |
| App access via SSO | Okta → App-to-user mapping | Okta Apps API |

> **Key insight:** If all AI tools are configured to use **SSO (Okta/Azure AD)**, you get user-level attribution for free from the IdP, without needing per-tool integrations.

---

### 3. Shadow AI Detection (Unapproved Tool Discovery)

| Method | Source | What it catches |
|---|---|---|
| **DNS / Web proxy logs** | Zscaler, Netskope, Palo Alto NGFW | Any domain the tool uses (e.g., `perplexity.ai`, `runway.ml`) |
| **DLP / CASB alerts** | Netskope, Microsoft Defender for Cloud Apps | File uploads, data-sharing events to unapproved AI tools |
| **Browser extension** | Custom or CrowdStrike Falcon | Tab URL tracking with employee consent |
| **Expense reports** | Concur / Expensify webhooks | Personal card charges for AI subscriptions |
| **Email receipts** | Gmail / Outlook API (filtered for AI tool invoices) | Shadow subscriptions paid personally |

---

### 4. Budget Limits & Policy Rules

| Data Point | Source |
|---|---|
| Per-tool monthly spend limits | Internal config DB (set by Finance / IT admin in the dashboard itself) |
| Approved tool list | Internal policy table (maintained by IT Security) |
| Alert thresholds (e.g., 80% warning) | Dashboard admin settings |
| Department budget allocations | Finance system (NetSuite, Quickbooks) via API |

---

## Data Pipeline Architecture (Simple)

```
[OpenAI API]  [Anthropic API]  [MS Graph]  [Okta]  [Netskope]  [Brex API]
       │              │              │          │         │           │
       └──────────────┴──────────────┴──────────┴─────────┴───────────┘
                                     │
                           [ETL / Ingestion Layer]
                        (Airbyte or custom Python workers)
                                     │
                           [Central Data Warehouse]
                            (BigQuery / Snowflake)
                                     │
                        [Transformation Layer (dbt)]
                                     │
                      [Dashboard API (FastAPI / GraphQL)]
                                     │
                       [Frontend Dashboard (React / HTML)]
```

---

## Refresh Cadences

| Data Type | Refresh Frequency | Rationale |
|---|---|---|
| Spend (approved tools) | Every 1 hour | Near-real-time cost visibility |
| User login / activity | Every 15 minutes | Fast shadow AI detection |
| DNS / proxy logs | Real-time stream | Immediate alerting on new tools |
| HR/team data | Daily | Org changes are slow-moving |
| Budget limits | On-change | Admin-triggered |

---

## Privacy & Compliance Considerations

- **Employee consent** required for browser-level tracking in most jurisdictions (GDPR, CCPA).
- **Data minimization**: Track app-level usage, not content of AI prompts.
- **Role-based access**: Managers see team aggregates; only IT Security sees individual user data.
- **Audit logs**: Every policy change and alert action must be logged.

---

## Fallback for Tools Without APIs

Some tools (e.g., Midjourney via Discord bot) have no official billing API.
Fallback options:
1. **Virtual corporate cards per tool** (Ramp, Brex) → cap spend at the card level
2. **Email invoice parsing** (GPT-4 on inbound receipts)
3. **Manual import** (CSV upload from tool's own billing page)

