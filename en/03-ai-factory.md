# 03. The AI factory: what it really is

> Written in tandem: Dzmitry Varabei and Claude Fable.
> August 14–21, 2026. One more review: September 3, 2026.
> [Читать по-русски](../ru/longread/03-ai-factory.md). Translated from the Russian original; the Russian version is canonical.

The words "AI factory" sound like a plant with magic inside. Let's take it apart down to its contents — and it will turn out there's nothing inside that you couldn't build yourself. That's the good news.

## The blunt question

One agent is useful, but delivery doesn't end with a single task: there's requirements analysis,
planning, implementation, tests, review, release. How do you assemble, out of the fundamental
concepts, a **process** that repeatably takes a task from ticket to PR? The construction that
does this is what we call an **AI factory** (the concepts are in [doc 02](02-fundamentals.md)).

*(Careful with the term when searching: in the public sphere "AI factory" most often means
something else entirely — data centers for training and running (inference of) models,
"factories that produce intelligence": that's how NVIDIA uses the term. Our meaning comes
from the world of agentic software delivery; the industry has no single established name for
this construction yet — similar things are called agentic pipeline, agentic SDLC, delivery
factory. On the projects this series is about, "factory" is EPAM's working term, not an
industry standard: it's marked that way in the [position requirements](requirements/ai-sdlc.md)
too.)*

## What a factory is, physically

Let's remove the wrapping. An AI factory is:

> **a repository of structured text files** (agents, skills, rules, commands)
> **+ a harness that executes them + a process with checkpoints.**

A typical structure looks roughly like this (folder names differ between harnesses):

```
.agents/          agent descriptions (who does what, which skills they call)
skills/           atomic steps of work (concept 5, doc 02)
rules/            project rules: conventions, constraints, style
commands/         entry points: "take a ticket", "do a review"
docs/evidence/    proof: test logs, check reports
```

The role of "project rules" in real repositories is most often played by a file in the root:
**CLAUDE.md** (read by Claude Code) or **AGENTS.md** — an open standard not tied to any
specific model or harness; often the first one simply points to the second. The rule for what
goes in is simple: **the primary documentation is the code**; these files record only what
cannot be read from the code — conventions, constraints, and the architectural decisions that
were made. The agent itself can write a draft of such a file by scanning the repository — and
the team edits and maintains it.

No magic: **everything that makes a factory a factory is text, thought through and written
into files**. There are enough open samples: **Superpowers** (an open skill-first framework
for agentic development: spec → plan → TDD implementation (test-driven development: test
first, then code) → review, all built on skills) and **Agent Skills** (Anthropic's open
standard: a skill as a folder with a description that the harness loads on demand) — both can
be read like source code. The difference between teams is in the quality of the texts and the
discipline of the process.

Hence the answer to a frequent question: **"why some factory, when there's BMAD, OpenSpec,
spec-kit?"** The question hides a category error: a factory is not a competitor to these
frameworks but the name of the class of constructions they all belong to. **BMAD** is a
ready-made factory scaffold with role agents (product, architecture, testing) and a cycle of
"clarify → plan → build and verify"; **OpenSpec** and **spec-kit** are tools of the spec layer
(SDD, [doc 02](02-fundamentals.md)); Superpowers is a skill-first variant of the same
construction. Take any of them — but what makes the scaffold a factory is not the choice of
framework: it's the content grown for your project, and the team's discipline. Frameworks
change every six months; the foundation under them is one and the same — whoever understands
the basic concepts isn't tied to any of them.

The factory connects to external systems via **MCP** (Model Context Protocol) — an open
standard through which the agent gets data and tools from the issue tracker, design files,
knowledge bases. The image: USB-C for AI applications — one connector instead of a
hand-written integration for every system.

The consequence: **you can build a factory yourself, on any stack, starting from an empty
folder.** The whole question is whether you understand what to write into the files.

## How it works: the flow of a task

Inside the factory a task flows along a pipeline:

```
ticket → spec → plan → critic → gate: a human approves the plan
       → implementation → checks → evidence → PR → gate: a human accepts
```

- An **orchestrator agent** classifies the input and picks the route (a bug takes the short
  path, a feature the full one).
- Every step is a skill or an agent with its own narrow job.
- **Critics** stand between the steps and check the output against the spec — before any human.
- **The human stands at the gates**: approves the plan, accepts the PR. The agents prepare the
  decision — the human makes it. This is called human-in-the-loop, and in serious production
  development human approval of the key steps remains the norm; the depth of control depends on
  the cost of a mistake and the reversibility of the change. There are plenty of promises that
  "soon the human won't be needed"; working examples in serious development that we know of —
  none.

## The factory's two outputs (this is the main point)

It's naive to think a factory produces only software. A good factory has **two outputs**:

1. **Delivered software** — closed tasks, features, releases.
2. **Reusable artifacts** — new and improved skills, rules, specs, recorded decisions. The
   things that make the *next* task cheaper and faster.

In the long run the second output matters more than the first — under one condition: **future
tasks resemble past ones** (otherwise the library doesn't transfer). For a team living in one
product or one class of tasks, the condition holds almost always — and then the arithmetic is
simple: the software is sold once, while every skill works for all the tasks that follow. A
team that closes tickets but doesn't grow the library is just working fast. A team where every
task leaves an artifact behind is **accumulating** — and the gap between them grows with every
month.

The same holds for a person: if after every task you're left with a reusable file (a skill, a
spec template, a checklist), your personal "factory" grows. That is your portfolio — not
certificates, but working artifacts.

## "I can do all of this in one Claude Code session. Why a factory?"

You can. And for a small task that's exactly what you should do (see "When you don't need a
factory" below). The difference shows over time — not in *what* is done, but in *what it
rests on*:

- **In a session, the process rests on your memory.** The spec, the tests before commit, the
  second look at the diff — all of it exists only if you remembered it today. In a factory the
  checkpoints are baked into files and hooks — they fire regardless of what kind of day you're
  having.
- **In a session, "done" is the agent's word.** In a factory — evidence in the repository.
- **A session evaporates.** Everything you figured out and set up in it dies with its window.
  A factory keeps the second output (see above) — and the next task is cheaper.
- **A session is personal.** Ten engineers in personal sessions are ten different processes; a
  factory is one process per team.

In short: a session is you plus an agent. A factory is a process that works the same no matter
who the operator is and no matter what they forgot. The same dividing line as in
[doc 01](01-economics-and-engineer-role.md): the boundary runs along whether the verification
is systematic.

## What a factory is NOT

- **Not an automaton without people.** Fully autonomous delivery is sometimes called a "dark
  factory" (by analogy with plants where you can switch off the lights). As far as we know, this
  really works only where the cost of a mistake is small. In real development a human
  approves the key steps.
- **Not a box that gets "installed".** Setting up the folder structure is an hour of work. What
  makes it a factory is content grown for the specific project, and a team that knows how to
  work with it.
- **Not a substitute for understanding.** An operator who doesn't understand what's happening
  under the hood can neither fix the process nor notice that the agent is confidently doing the
  wrong thing.

## When you don't need a factory

The rule is simple:

```
large / repeatable task, unfamiliar code      → the factory is worth it
small task / you know exactly what to do      → your hands, or one agent with no process
```

Running the full pipeline to rename a class is the classic mistake of a beginner with a new
tool: "when you have a hammer, everything looks like a nail." Knowing when *not* to use the
process is as much a skill as knowing how to use it.

## The analogy: a professional kitchen

One cook with a good knife (an engineer with an agent) can prepare a dish. A restaurant with a
hundred tables is not "a very fast cook" — it's a **kitchen**: stations with narrow jobs
(skills), recipe cards (specs), a sous-chef who tastes every dish before it goes out (the
critic), the head chef who gives the final approval (the human at the gate) — and the main
asset: **a recipe book that keeps growing**. A
cook leaves — the kitchen keeps cooking, because the knowledge is in the recipe cards, not in
someone's head.

## Check yourself

1. A team is being sold an "AI factory, turnkey, installed in a week." What in this offer is
   consistent with how a factory works, and what contradicts it? Which two questions would you
   ask the seller?
2. Team A closed 120 tickets with agents in a quarter; its skill library is empty. Team B
   closed 80, but has 25 proven skills and project rules. Who is in the better position,
   and under what condition would your answer flip?
3. A team lead proposes removing the human gate on PRs: "the critic agent checks everything
   anyway." What distinguishes a critic from a gate, and what exactly would the team lose?
