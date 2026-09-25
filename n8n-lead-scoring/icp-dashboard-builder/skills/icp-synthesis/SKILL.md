---
name: icp-synthesis
description: Turn a B2B CRM export (won and lost accounts, their contacts, public signals and verbatim notes) into a grounded ICP at account and buying-group level, with an anti-ICP. Use before planning campaigns, nurture or routing.
type: skill
---

# ICP Synthesis (B2B)

> Adapted from the `icp-synthesis` skill by Nick Christensen (ship-icp-ads-automate-monitoring,
> MIT License, see `../../THIRD-PARTY-NOTICES.md`). Changes: best customers defined by retained and
> expanded ARR instead of top revenue; contrast with average, churned and lost accounts; account
> plus buying group instead of a single avatar; anti-ICP.

You turn a CRM export into an ICP that is real enough to plan campaigns from. Not a demographic,
not a stock persona. A type of account you could name, and the people inside it you need to win.

## The core rule

The data is the **only** source of truth. Company, footprint, role, the words people use: every
field must be grounded in a real signal from the data or enrichment. If there is no signal, leave
it blank. **Empty cells beat hallucinated cells every time.** A confident guess that's wrong
poisons every campaign, sequence and routing rule downstream.

## Step 1: Find the best customers

In B2B each account signs one contract, so ranking by revenue shows who signed the biggest deal,
not who is the best customer. A customer that signed a large contract and left at renewal is a
cost, not a model.

Best customers are won accounts that stayed and grew: compare current ARR with the ACV signed.
Look at what they have in common: size, footprint, the situation they were in, how they came in.

## Step 2: Contrast, then map the segments

A trait belongs to the ICP only if it separates the best customers from the rest. Compare them
with average customers (won, flat), wrong-fit customers (won, then churned or contracted) and
lost deals. A trait as common among lost deals as among best customers is not an ICP trait.

Then name the segments below the ICP, and the anti-ICP: who to exclude from targeting and routing,
and why. This keeps you honest about who you're choosing to ignore.

## Step 3: Enrich with public signals

For each account, add what's verifiable in public: acquisitions, expansion into new countries,
contract renewals, leadership changes, tenders. Tier your confidence and mark which tier each
signal came from. Signals tell you timing: what was happening at the account before it entered
pipeline.

## Step 4: Layer the buying group and the qualitative

Accounts tell you which companies. Contacts tell you who inside: which roles show up, who
champions, who decides, who blocks, and how that differs between best customers and lost deals.

Words tell you why. Pull from call notes, win/loss interviews and reviews. This is where the
buying motivation and the real objections live.

## Step 5: Keep their exact language

When you capture how a customer talks, **keep grammar, slang, and phrasing intact.** Do not
clean it up. Do not paraphrase. The moment you smooth "I stopped being a part-time procurement
manager" into "reduced vendor management overhead," you've thrown away the only thing that makes
the copy convert. People buy words that sound like their own thoughts.

## Step 6: Cross-validate

Check the synthesized ICP against the calls and the win/loss interviews. Does the account on the
call match the account on the page? If not, the data lied to you somewhere. Phone calls stay
irreplaceable: AI tells you who to call, not what they'll say.

## Step 7: Compile into a reusable profile

Write the result to disk so every downstream step (campaigns, nurture, routing) loads the same
ICP on every run. The synthesis is only valuable if it persists and compounds.

## Output

A single ICP dossier (one page): the ICP at the top (account and buying group), the segments and
the anti-ICP below, a "their words" section of verbatim language, and the pains, desires and
triggers. Each grounded, each traceable to a signal in the data.
