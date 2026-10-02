# 01. AI writes the code. So what does the engineer do now?
> Written in tandem: Dzmitry Varabei and Claude Fable, critiqued by GPT Sol.
> August 13, 2026. Writing and refactoring time: 6+ hours
> August 17, 2026. 1 hour
> [Читать по-русски](../ru/longread/01-economics-and-engineer-role.md). Translated from the Russian original; the Russian version is canonical.

## How the custom software development market has worked for the last 30+ years

Below are four steps about the economics of software services companies (heavily simplified!): how the **unit of production** gradually changes at the company level, and, at the engineer's level, **their role inside it**.

### Step 1. An outsourcing company at the level of physics

Let's remove everything extra. A custom software development company (software engineering services) is a machine that **turns engineers' mental work into money**. The unit it sells to the client is **an hour of a specialist's work**.
The model is literally called that: **T&M (Time & Materials)** — "pay for time spent." Revenue =
number of engineers on projects × rate × hours.

The beauty of the formula is that everyone understands it instantly: the salesperson, the client, and the engineer filling out a timesheet on Friday evening. Other models exist, and there are many — fixed price, for example — but the hourly one has stayed the foundation of custom development for thirty years, especially where the scope of work is unknown in advance.

For specifics: at EPAM, hourly billing still remains the primary unit — the company's publications say that most revenue comes from Time & Materials contracts with hourly, daily, or monthly rates. But the share of this model is noticeably declining: 87.0% of revenue in 2023 → 82.5% in 2024 → 80.0% in 2025. Over the same period, the fixed-price share grew from roughly 12.3% to 19.5%.

### Step 2. The assumption that value delivered ≈ proportional to hours spent

The T&M model is built on one assumption:

> **value delivered ≈ proportional to the number of hours spent.**

This holds as long as software is made by hand. Want twice as many features — you need roughly twice as many person-hours.

The second reason to hold on to hours is risk: the scope of software work is hard to estimate up front, and selling time is safer than promising an outcome.

### Step 3. What exactly does the LLM change?

The device that changes all of this is called an LLM-based agent.

Imagine a regular T&M project. A ticket has a two-day estimate, and it's an honest one. The engineer puts an agent on the task — and closes it in an hour and a half. Nice. For the engineer, that's great news. And in the evening, the delivery manager puts together the weekly report and sees the same news from the other side: minus fourteen hours off the client's invoice. One such ticket is nothing — easy to ignore. But the engineer is not going to stop at one ticket.

**One note.** The effect of using AI isn't always stable. Projects differ and tasks differ. But overall it's enough that **a large class of tasks has appeared where the "hours ↔ value" proportion is broken** — and after that the chain moves on its own.

And here is the problem for our software engineering companies. If you still sell *hours*, adopting AI **destroys your own revenue**: for the same work you can now honestly bill several times fewer hours. The better your AI — the less you earn. **The business model itself pushes the company away from adopting AI.**

### Step 4. One way to solve the problem: change the unit of sale

If selling hours has become unprofitable, you can **change the very unit of sale**: stop selling *time* — start selling *outcomes*, a delivered unit of work (a closed ticket, a feature, a release) with a quality commitment.

This shift reverses what AI means for the business. In the "sell the output" model, a unit that's cheap to produce = **higher margin**. Now every hour the agent saves works *for* you, not *against* you: the same hour and a half instead of two days — but in the delivery manager's report it's no longer a problem, it's profit.

With one note: the unit's cost isn't just coding hours — it's also tokens, review, rework, and a risk reserve; speeding up coding by 80% won't give you −80% cost. But the general trend is clear.

**Wait — why wasn't this done thirty years ago?** The fixed-price model has existed for a long time — but was never especially popular. The reason: selling outcomes shifts the **risk of estimation error** onto the seller. Software development is hard to estimate, and at a fixed price every underestimate is the vendor's direct loss; that's why vendors prefer T&M.

The agent doesn't eliminate this risk, but it provides two tools against it. The first is a **cheap test run**: often for a few dollars you can make a trial attempt and *measure* the task before naming a price; before, even the estimate itself cost too much. How well such a spike predicts the full delivery is a question for your own telemetry — which brings us to the second tool. The second is **telemetry**: every closed task leaves a record of "how much it cost and how long it took," and the estimate for the next one rests on your own statistics instead of someone's intuition on a bad day. Estimation risk doesn't disappear — it becomes manageable, like the risk of an insurance company, but only for those who measure their work with discipline. That is why (looking ahead) these docs care so much about cost journals and metrics: in the output model, the ability to count is a condition of survival.

The shift itself can be described as moving from **executor** (produces the unit of work themselves) to **human orchestrator** (assigns tasks to agents, runs the process, verifies and accepts the result).

As of August 2026, the human orchestrator is usually **not yet a manager of AI agents**. It's an engineer who is capable of doing the work themselves but uses AI to write the code, while remaining the technical owner of the result. Remove "engineer" from that sentence — and the whole thing falls apart: you cannot own a result you cannot verify. So "programming without knowing how to program" doesn't work yet.

## An analogy, just in case: the tailor and the sewing machine

A tailor has been sewing for twelve years and charges **for hours of hand stitching**. In spring, a sewing machine appears in the workshop across the street — owned by a neighbor who has been sewing for three years. The same garment is now sewn several times faster.

- If he **keeps charging for hours** — the machine kills his income (there are fewer hours), and buying one is unprofitable for him.
- If he **refuses the machine** — the neighbor sets a price *per garment* and takes his clients away. Not because she sews better: because she can name a price he can't.
- The only way out: **charge per garment with a quality guarantee**. Then the machine is pure margin, and the better it is, the richer he gets.
- And the tailor himself stops stitching by hand and becomes the one who *sets up and supervises* the machines and answers for the result.

And now the place where this analogy breaks. A sewing machine doesn't make mistakes *confidently and plausibly*: a crooked seam is visible against the light in a second. An agent makes mistakes exactly that way — it produces something that looks right instead of something that works, neatly, with tidy indentation and a convincing comment about why it was done this way. So the tailor from the last bullet is a simpler figure than a real agent operator: for a real operator, half the job is **verification**.

## What if I'm not in outsourcing — I'm building my own product?

The chain above was derived for services companies — there, the change of sales model is forced. In product companies, startups, and freelancing, hours were never the thing being sold, and the driver is different and simpler: **competition on speed and cost of delivery**. A team shipping features through an agentic pipeline releases faster and cheaper than a team working by hand — and that puts pressure on everyone who writes software, regardless of the sales model. Different paths, same conclusion: value shifts to those who can run agentic delivery.

**"I'm already vibe-coding my product — why learn this?"** (vibe coding — a working style where the agent's output is accepted at a glance, without reading or verification). First, let's be honest: for a prototype, a one-time script, a weekend landing page, vibe coding works, and there isn't much to learn there — if the code dies in a week, there's no point verifying it. The problem is that weekend landing pages sometimes don't die: six months later you have paying users, a file upload form, and a table with other people's addresses inside.

Everything breaks exactly at that moment: the cost of a mistake stops being zero, and the agent — remember where the tailor analogy breaks — makes mistakes *confidently and plausibly*. Vibe coding is exactly that: accepting what looks right without checking it; on a live product, unchecked code piles up silently until it blows up — usually in security, in data, or in the token bill.

The label, however, is misleading — in a good way. If you call yourself a vibe coder but run tests, check previews, and watch production errors — you're already half an operator, just without a system; the boundary runs not through the word, but through whether **verification is systematic**. The vibe coder and the operator sit at the same agent — the difference is that the operator **answers for the result**. It's the same Step 4, only the "client" is now your own users; your own product doesn't free you from the operator role — it concentrates it: you are the customer, the vendor, and the validator in one person. So if you're already vibe coding — you've felt the speed; the program teaches the second half of the craft: answering for what that speed produces. How this same boundary looks at the level of process and team — the question "why a factory if there's a session" — is in [the AI Factory doc](03-ai-factory.md).

## Will there be fewer engineers? Our bet is no — and here's why

Steps 3–4 lead to something unpleasant: the **same** volume of work needs fewer person-hours. If the volume of work were a constant, fewer engineers would be needed — period. That's a direct consequence, and pretending it doesn't exist would be dishonest.

Our bet on the opposite rests on a missing piece — the **elasticity of demand for software**: when the output gets several times cheaper, things that never used to pay off become profitable. Automation for small businesses, internal tools, niches where it made no sense to bring in developers at $X per hour — that hidden demand opens up and takes in the freed-up engineers. This has already happened with programming itself: compilers, frameworks, and the cloud each "cut the work" — and each time the number of engineers grew, because cheap development opened new applications.

**The radiologists story.** In 2016, Geoffrey Hinton — the "godfather" of deep learning, later a Nobel laureate — publicly declared it was time to stop training radiologists: within five years, neural networks would read scans better than humans. It was said about the profession, not just the scans, and it sounded final. Ten years later, AI really is built into almost every radiology platform — but it reads scans *together with* the physician, not instead of them, and there are now **more** radiologists: the shortage remains, and the number of scans grows faster than the number of doctors. One of the mechanisms is that same elasticity (the growth has many causes: demand, demographics, regulation — but this is the lesson that matters for us): reading a scan became cheaper and faster → more scans get ordered, diagnostics reaches cases that no one had the capacity for before → more people *responsible for the diagnosis* are needed — they just now work together with the model
([transcript at Lex Fridman](https://lexfridman.com/jensen-huang-transcript/),
[CNBC](https://www.cnbc.com/2025/12/04/jensen-huang-cited-radiologists-to-dispute-ai-jobs-impact.html),
[Fortune](https://fortune.com/article/ai-godfather-radiologists-obsolete-salaries-up-to-571k-demand-growing/)). The main thing this story breaks is not
"automation creates jobs," but the opposite, too-simple idea: "AI automated part of the task — therefore the profession will disappear."


## What this means for a junior

**The bad news.** The classical entry into the profession is under pressure: the routine tasks a junior traditionally grew on are the first to go to agents. How much this will shrink the number of junior positions, nobody has reliably measured yet — but the direction of the pressure leaves no doubt. "Just learn a framework" works worse and worse as an entry strategy — and a graduate who honestly learned a framework and built four projects with it has every right to consider that unfair. It is unfair. What follows is about what to do with it.

**What you now need to be able to do.** The role of an agentic-delivery operator is built from four skills:

1. **delegate** — break the task into parts, describe it to the agent in writing (spec), hand the machine what it will do better and cheaper;
2. **supervise** — run the agentic process: checkpoints, early stopping, stepping in by hand when the process goes off track;
3. **evaluate** — the main skill: telling code that works from code that only looks right, and measuring the cost and quality of your own work in numbers;
4. **improve** — turning every solved task into a reusable artifact: a skill, a rule, a template.

**Fundamentals decide.** All four skills stand on knowledge of the base stack and an understanding of how applications work.
Validation grows out of the ability to read other people's code — **without fundamentals you have nothing to verify with**, and no agent compensates for that. So building yourself a solid foundation is a hard requirement, not a formality. Fortunately, you can build all of this through regular practice and solving many different tasks.

## Off-topic from the author

I try to look at life with optimism and I'm sure that almost anything can be looked at from different angles to find something good or funny in it. A minute ago I saw an ordinary spider crawling on my balcony — nothing surprising. But since I recently started reading *Project Hail Mary*,
in that same spider I saw Rocky, the alien engineer from the 40 Eridani system, and that most ordinary spider made me smile and stopped being ordinary. The spider didn't change; the way of looking did. And we can simply walk into a store and buy a pineapple — something my ancestors couldn't even dream of.
This text was written together with an AI agent — that fact alone was science fiction until recently.
We live in an amazing time.

<!-- TODO: closing navigation — links to the next docs in the series -->
