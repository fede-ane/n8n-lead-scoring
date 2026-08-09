# Lead scoring and routing

An n8n workflow that scores and routes inbound leads from a webinar registration form.

![Workflow canvas](canvas.png)

## Why it's built this way

Most lead scoring weights seniority heavily. That works when a product is bought top-down.

It works less well for an open-source product whose free tier has no usage limits. Nobody buys because they need the capability — they already have it. They buy when scale creates a governance problem: audit logging, RBAC, multi-team separation, or a compliance requirement.

So this model weights **governance signals above firmographics and above seniority**. A platform engineer running fifty workflows on a self-hosted community edition scores higher than a VP who downloaded a report.

## Flow

```
Form → Normalize → Fetch website → Extract signals → Classify (LLM)
     → Score → Route ─┬─ High fit ──→ AI brief → Slack
                      ├─ Emerging fit ──┐
                      └─ Out of ICP ────┴─→ Google Sheet
```

The LLM returns categories, not numbers — models are unreliable at consistent numeric scoring across runs, but reliable at assigning categories. The Code node converts those categories to points deterministically. When enrichment fails the model returns `unclear` rather than guessing.

## Scoring

| Component | Max | Inputs |
|---|---|---|
| Firmographic | 30 | Size band, ICP fit from LLM |
| Governance | 45 | Deployment type, workflows in production, regulated sector |
| Seniority | 25 | Job title keywords |

Thresholds: ≥70 High fit, ≥40 Emerging fit, below that Out of ICP.

Self-hosted community scores higher than a paid Business plan — the model measures opportunity, not current value. Tiers are named for fit rather than MQL/SQL, which mean purchase readiness and are a different thing.

## Sample output

| Company | Size | Deployment | Workflows | Score | Tier |
|---|---|---|---|---|---|
| Global telco | 1000+ | Self-hosted community | 50+ | 100 | High fit |
| Digital bank | 201–1000 | Cloud plan | 1–10 | 58 | Emerging fit |
| Small consultancy | 1–50 | Evaluating | None | 11 | Out of ICP |

The middle row is the interesting one: strong ICP fit, regulated sector, right size — but no governance pressure yet. Correctly held back from sales handoff.

## Limitations

Weights are initial estimates and would need recalibrating against ninety days of conversion data.

Enrichment scrapes the company homepage, which fails on sites that block server-side requests. In production this would come from a data provider or the CRM.

A lead whose website could not be fetched still accumulates points from self-declared form fields, which are unverified.

## Setup

Import the JSON, add credentials (Google Gemini, Google Sheets, Slack), replace `YOUR_SPREADSHEET_ID` in the Log lead node, and create a sheet with these headers:

```
submitted_at, email, domain, company_name, job_title, size_band,
deployment, wf_band, industry, icp_fit, regulated_sector,
score_firmographic, score_governance, score_seniority, score,
fit_tier, ai_reasoning
```

Credentials and instance identifiers have been stripped from the export.
