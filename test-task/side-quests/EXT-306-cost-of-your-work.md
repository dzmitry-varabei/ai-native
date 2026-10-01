# EXT-306 — What Your Work Cost (after the final report)

> Optional side quest · both tracks · not part of the main route
> Rules: [test task](../README.md) · Quest map: [side-quests/README.md](README.md)

**Why:** on real AI-native projects inference is a cost line on the order of 5% of revenue, and a task of average complexity costs tens of dollars; an operator must see the price of their work — at the implementation stage it is literally the main thing they control.

**What it trains:** reading a run's tokenomics, attributing cost to stages, the judgment call of "where it was expensive and why".

**The real problem behind the quest:** production bootcamps run an early-stop rule — if a small task has already eaten $10 at the spec stage, it gets stopped and taken apart; without the habit of looking at the price, that rule never fires.

**The task:** using the analytics report (`report/codemie-analytics-<date>.report.json`) and the `/usage` data from your sessions, break down the cost of your route: what each task cost; within one spec-driven task — how much went to the spec, the plan, the implementation; compare an average free-form task with a spec-driven one.

**Definition of Done:**
- [ ] `docs/tokenomics.md`: a table of "task → sessions → tokens → cost estimate"
- [ ] For one spec-driven task — a breakdown by stage
- [ ] Three conclusions and one concrete proposal for what you would do cheaper
