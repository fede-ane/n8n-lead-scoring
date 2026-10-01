# Build prompt 2: the campaign

Adapted from the build prompt by Nick Christensen (ship-icp-ads-automate-monitoring, MIT License,
see `THIRD-PARTY-NOTICES.md`). Paste the prompt below into Claude Code with this folder (`icp/`)
open. It produces one page you can open in a browser: an example campaign built on the ICP
(`output/campaign.html`).

**Run this only after [`BUILD-PROMPT-1-ICP.md`](BUILD-PROMPT-1-ICP.md)**, and only once you have
read and checked `output/icp-dossier.html`. The campaign is built entirely on the dossier.

> **The data behind the dossier is synthetic.** Fictitious companies, people and figures. See
> [`README.md`](README.md).

Fill in your landing page and brand, or leave them blank and Claude will use a placeholder brand
for the demo. It can run in the same Claude Code session as prompt 1 or in a new one: it reads
the dossier from `output/`.

## The prompt

```
Read voice/voice-rules.md and skills/ad-copy/SKILL.md in full before doing anything. Your source
is output/icp-dossier.html.

PASS 2: BUILD AN EXAMPLE CAMPAIGN (use the ad-copy skill)
- Target the ICP. Landing page: [YOUR LANDING PAGE URL] · Business name: [YOUR BRAND].
  (Leave these blank and use a fictitious placeholder brand for the demo.)
- Generate Google Search RSA assets: 15 headlines (<=30 chars) and 4 descriptions (<=90 chars).
- Generate LinkedIn Sponsored Content for each role in the ICP buying group: 3 introductory
  texts and 3 headlines (<=70 chars) per role, each built on the reason that role buys.
- Generate the audiences: the LinkedIn account list and job titles per role, the exclusions from
  the anti-ICP, Google keyword seeds and negative keywords.
- SCORE every asset against the ICP (1-5) on: does it use their words, does it lead with the
  outcome, does it pass the voice rules. Show the score next to each line. Cut anything under 4
  and say why.
- Output output/campaign.html: a separate page with the scored copy grouped by platform and
  role, and the audience plan.

Lead with the buyer, not the product. Outcomes over features. When you're done, give me a
5-line summary and the file path.
```
