# Build prompt 1: the ICP

Adapted from the build prompt by Nick Christensen (ship-icp-ads-automate-monitoring, MIT License,
see `THIRD-PARTY-NOTICES.md`). Paste the prompt below into Claude Code with this folder (`icp/`)
open. It produces one page you can open in a browser: the ICP dossier
(`output/icp-dossier.html`).

This is the first of two prompts. Read and check the dossier, then run
[`BUILD-PROMPT-2-CAMPAIGN.md`](BUILD-PROMPT-2-CAMPAIGN.md). The campaign is only as good as the
ICP it is built on.

> **The data in `data/` is synthetic.** Fictitious companies, people and figures, generated for
> this demo. See [`README.md`](README.md) for what the demo shows and what it does not.

### The data

Do not rename the `data/` folder or its files: the prompt refers to them by name.

- **`data/accounts.csv`** (required): one row per account, won and lost. Minimum columns:
  `account_id`, `outcome`, `acv_gbp`, `current_arr_gbp`.
- **`data/contacts.csv`**: one row per person, linked by `account_id`, with `buying_role`.
- **`data/enrichment.csv`** (optional): public signals per account. If the file exists, the
  prompt uses it and does not search the web: the demo companies are fictitious. If it does not
  exist, the prompt researches public signals on the web.
- **`data/call-notes.md`, `data/win-loss-interviews.md`, `data/reviews.md`**: verbatim language.

### Bring your own data

Replace the files in `data/` with a real export and keep the names. Delete
`data/enrichment.csv` and the prompt will research public signals on the web instead. The prompt
itself does not change. Full details in [`README.md`](README.md).

---

## The prompt

```
Read voice/voice-rules.md and skills/icp-synthesis/SKILL.md in full before doing anything.
Your source data is the data/ folder, plus web research only in the case described below:
- accounts.csv            (won and lost accounts: firmographics, source, intent,
                           dates, ACV, current ARR, lost and churn reasons)
- contacts.csv            (people per account: title, department, seniority, buying role)
- enrichment.csv          (optional: public signals per account)
- call-notes.md           (discovery and demo calls)
- win-loss-interviews.md  (why they chose us, or didn't)
- reviews.md              (customers, after purchase)
Enrichment: if data/enrichment.csv exists, use it as the only source of public signals and do
not search the web (the demo companies are fictitious). If it does not exist, research public
signals for each account on the web (acquisitions, new countries or sites, contract renewals,
leadership changes, tenders), and record the source, date and confidence tier of each.

PASS 1: SYNTHESIZE THE ICP (use the icp-synthesis skill)
- Define the best customers as won accounts that stayed and grew: compare current_arr_gbp with
  acv_gbp.
- Compare them with average customers, customers who churned or contracted, and lost deals.
  Keep only the traits that separate the best customers from the rest.
- Describe the ICP at two levels: the account (firmographics, current setup, what was
  happening at the account before it entered pipeline) and the buying group (which roles are
  involved, who champions, who decides, who blocks). Give the profile a short name.
- Identify the other segments and the anti-ICP: who to exclude from targeting and routing, and
  why.
- Cross-reference the best accounts and their contacts against call notes, win/loss interviews
  and reviews, and pull their exact words. Keep grammar and slang intact. Never paraphrase real
  phrasing into corporate-speak. If a field has no real signal, leave it blank: empty beats
  hallucinated.
- Output output/icp-dossier.html: a clean single-page dossier. The ICP up top, then the segments
  and the anti-ICP, then a "their words" section of verbatim language, then the pains, desires
  and triggers.

When you're done, give me a 5-line summary and the file path.
```

---

## What "good" looks like

- The ICP is **a type of account you could build a target list from**, pulled from the data,
  with the people inside it named by role.
- Every trait **separates the best customers from the rest**. Nothing that is equally common
  among lost deals.
- Gaps **stay blank**, and the words come from the customers.
