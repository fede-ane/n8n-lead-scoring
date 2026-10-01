# B2B ICP: from a CRM export to a grounded ICP and a scored campaign

**Outcome:** a CRM export of won and lost accounts becomes an ICP you can build a target list
from: the type of account, the people inside it, the triggers that bring it into market, and
the accounts to keep out. That ICP then becomes an example campaign, graded against the buyer
before a single line ships.

> **All data in `data/` is synthetic.** It describes a fictitious B2B company and its fictitious
> customers. Every company, person, quote and number was generated for this demo. None of it
> describes a real company, customer or deal, and none of it is research about any real market.
> The data was generated from business assumptions written for the demo, so the ICP that comes
> out can only confirm them. What the demo shows is the **method**: whether
> it finds the signal, avoids the traps and leaves gaps empty. It does not show what the real ICP
> of any company is.

---

## Run it (the demo)

The synthetic data is already in `data/`. Nothing to download.

1. **Open the `icp/` folder itself** in VS Code (File > Open Folder) or in a terminal
   (`cd icp`). Not the parent folder: the prompt uses paths relative to `icp/`.
2. In VS Code, if a "Restricted Mode" bar appears, choose **Trust**. The Claude Code extension
   does not run in Restricted Mode.
3. Start Claude Code: the extension (Claude icon, or Cmd+Esc), or `claude` in the terminal.
4. Open [`BUILD-PROMPT-1-ICP.md`](BUILD-PROMPT-1-ICP.md), copy **only the text inside the code
   block**, paste it into Claude Code and press Enter.
5. Approve file reads and the write to `output/`. With `enrichment.csv` present, the prompt
   tells Claude not to search the web, because the companies do not exist: decline if it tries.
6. Open `output/icp-dossier.html` **in a browser** (right-click > Reveal in Finder, then
   double-click, or `open output/icp-dossier.html` from the terminal). Clicking the link inside
   Claude Code opens the source code, not the page.
7. Read the dossier. If it holds up, run
   [`BUILD-PROMPT-2-CAMPAIGN.md`](BUILD-PROMPT-2-CAMPAIGN.md) the same way. It writes
   `output/campaign.html`: Google Search and LinkedIn copy for each role in the buying group,
   every asset scored against the ICP, and the audience plan.

A finished [`icp-dossier.html`](output/icp-dossier.html) is committed in `output/` so you can
see what comes out before you run it. A new run overwrites it.

### What to look for when it runs

- The best customers are **won accounts that stayed and grew** (current ARR well above the ACV
  signed), not the biggest contracts.
- Every trait **separates the best customers from the rest**. A trait that is just as common
  among lost deals is not in the ICP.
- The ICP has **two levels**: the account, and the buying group inside it (who champions, who
  signs, who blocks).
- There is an **anti-ICP**: who to exclude from targeting and routing, and why.
- Signals are read **by date**: what happened before an account entered pipeline is a trigger,
  what happened after is not.
- Gaps **stay blank**. Accounts with no notes, interviews or reviews are flagged, not described.

---

## The 7-step synthesis (what the skill does)

1. Find the best customers: won accounts that stayed and grew, by current ARR against ACV.
2. Contrast them with average, churned and lost accounts, then map the segments and the anti-ICP.
3. Enrich with public signals, with a confidence tier on each.
4. Layer the buying group (contacts) and the qualitative (calls, interviews, reviews).
5. Keep their exact language. Do not paraphrase.
6. Cross-validate against calls and win/loss interviews.
7. Compile into a profile on disk that every downstream step loads.

Phone calls stay irreplaceable. AI tells you who to call, not what they'll say.

---

## What changed from the original

| Original (repeat-purchase business) | This version (B2B) |
|---|---|
| Best customers = top ~10% by revenue | Best customers = won accounts with current ARR well above the ACV signed |
| One file of customers | Won **and lost** accounts, plus churned and contracted customers, as the contrast |
| A single avatar, a person with a name | An account profile plus the buying group inside it |
| Segments below the avatar | Segments plus an explicit anti-ICP |
| Enrichment by web research | Same step: web research by default; the demo ships `enrichment.csv`, which replaces it, because the companies are fictitious |
| Support tickets | Win/loss interviews |
| Pass 2 (ad copy) in the same run | Pass 2 runs separately, after the ICP has been checked |
| Google RSA + PMax, and Meta | Google Search RSA only, and LinkedIn: where B2B targeting by role and by account list happens |
| One set of copy for the avatar | Copy variants for each role in the buying group |
| Lookalike and interest audiences | Account list from the ICP, job titles per role, exclusions from the anti-ICP |

Unchanged: the grounding rule (empty beats hallucinated), exact customer language, the voice
rules, and the single-page HTML dossier.

---

## Point it at your own data

Replace the synthetic files with a real export. Keep the file names, or change them in the
prompt too.

- **`data/accounts.csv`** (required): one row per account, won and lost. Minimum columns:
  `account_id`, `outcome`, `acv_gbp`, `current_arr_gbp`. The rest lifts quality: use the
  firmographic columns that matter in your market. The method assumes recurring revenue, since
  best customers are defined by current ARR against the ACV signed.
- **`data/contacts.csv`**: one row per person, linked by `account_id`, with a buying role.
- **`data/call-notes.md`, `data/win-loss-interviews.md`, `data/reviews.md`**: your own verbatim
  language, or empty them. This is where the best copy comes from.
- **`data/enrichment.csv`**: delete it. Without it, the prompt researches public signals for
  each account on the web. To use signals you already have, replace its contents instead. The
  prompt does not need to change.

The grounding rule is the whole game:

> The data is the only source of truth. If there's no real signal for a field, leave it blank.
> Empty cells beat hallucinated cells every time.

---

## Limits

- **Synthetic and circular.** The data was generated from assumptions, so the dossier
  confirms them by design. It tests the method, not the market.
- **Small sample.** The demo dataset is small. Any segment under five accounts is an anecdote;
  the dossier should say so.
- **In-sample numbers.** Any targeting rule the dossier proposes is fitted and measured on the
  same accounts, so its precision is optimistic until tested on new accounts.
  
  Adapted from the ICP module of Nick Christensen's ship-icp-ads-automate-monitoring workshop
(MIT License, see [`THIRD-PARTY-NOTICES.md`](THIRD-PARTY-NOTICES.md)). The original is built
for a business where the same customers buy again and again, so it finds the avatar in the top
~10% by revenue. In B2B each account signs one contract, so this version changes what "best
customer" means, and what the ICP describes. See [What changed from the original](#what-changed-from-the-original).

---

## The mesh

The work lives in folders, not in a chat window. When the work is on disk, a new Claude Code
session picks up exactly where the last one left off.

```
icp/
├── BUILD-PROMPT-1-ICP.md       # prompt 1: the ICP dossier
├── BUILD-PROMPT-2-CAMPAIGN.md  # prompt 2: the campaign, run after checking the dossier
├── skills/
│   ├── icp-synthesis/        # turn a CRM export into a grounded B2B ICP
│   └── ad-copy/              # turn the ICP into scored Google Search and LinkedIn copy
├── voice/                    # the voice rules every output passes through
├── data/                     # synthetic CRM export (fictitious companies)
│   ├── accounts.csv              # won and lost accounts: firmographics, source, intent,
│   │                             # dates, ACV, current ARR, lost and churn reasons
│   ├── contacts.csv              # people per account: title, department, seniority, buying role
│   ├── enrichment.csv            # public signals per account (stands in for web research)
│   ├── call-notes.md             # discovery and demo calls
│   ├── win-loss-interviews.md    # why they chose us, or didn't
│   └── reviews.md                # customers, after purchase
├── output/
│   ├── icp-dossier.html      # Pass 1, a finished run, committed so you can see the deliverable
│   └── campaign.html         # Pass 2, generated from the dossier
└── THIRD-PARTY-NOTICES.md    # licence of the adapted material

