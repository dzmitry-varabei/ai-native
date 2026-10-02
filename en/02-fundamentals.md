# 02. What an "agent" is made of: six fundamental concepts

> Written in tandem: Dzmitry Varabei and Claude Fable, August 14–21, 2026.
> Edited by ChatGPT (GPT-5.6 Sol). One more review: September 3, 2026.
> Skip this doc if you already know the terms LLM, harness, agent, context, skill, and spec.
> [Читать по-русски](../ru/longread/02-fundamentals.md). Translated from the Russian original; the Russian version is canonical.

Back in 2023, even the strongest LLMs often failed at hard high-school-level math problems.
In 2024, specialized Google systems reached silver-medal level at the IMO (International Mathematical Olympiad) for the first time.
In 2025, several systems reached IMO gold level, and Gemini performed at the level of second place in the world at the ICPC finals (International Collegiate Programming Contest).

By mid-2026, models are solving mathematical and physical problems whose meaning I don't understand myself, but which professional mathematicians and physicists had worked on for decades: problems from the Erdős list, a counterexample to the Jacobian conjecture, new results around the Riemann hypothesis, problems on the gravitational-wave spectrum of cosmic strings. The scale of this breakthrough is hard for me to grasp. But what makes me happy is how accessible knowledge has become: an LLM explains difficult concepts in simple words to anyone who asks. More on that in a separate off-topic at the bottom of this document.

Our task is simpler — to organize the work of an LLM properly for solving business problems. And there humanity still has an entire ocean of work to do.

## Concept 1. LLM — a consultant on the phone

An **LLM** (large language model) does exactly one thing: it takes input text and, step by step, continues it with its answer. That is enough to reason, write code, analyze documents, and build plans.

It can also write in its answer: "this tool needs to be called with these arguments." But it does not call the tool itself — an external program does that.

**The LLM itself keeps no state between calls.** The conversation history and the results of previous actions are stored by an external system — which passes them to the model anew on every call.

In an extremely simplified view, a local LLM is a huge file with billions of number-weights and a relatively small program that runs the input text through those numbers and computes the next token step by step.

```text
Llama 3 8B

model weights   ~ 4–16 GB, depending on quantization
llama.cpp       ~ megabytes of code/binaries
prompt          ~ kilobytes
```

What an LLM can **not** do on its own:

* **execute** a tool call — it only says "call this"; there is nobody to press the button;
* **remember** between runs — every call starts from scratch;
* **act** in the outside world — read files, run tests, make commits, reach the network.

The image: **a consultant on the phone**. Knows a great deal and will dictate what to do — but won't press a single button. And once the call ends, the conversation is forgotten: on the next call you'll have to retell it.

**What's missing:** one LLM is already useful, but **it won't carry out a business process by itself** — it has nothing to act with.

## Concept 2. Harness — a workplace for the model

A **harness** (the "rig" around the model) turns a talking model into a working one. The minimum it provides:

* **a loop**: give the model input → read the answer → if it asks for a tool — call it → return the result → repeat;
* **tools** — functions the model can ask to have called: read a file, run tests, perform a search, make a commit;
* **files and other context sources** to read;
* **working state** — the history of actions, the results of previous steps;
* **a stopping rule** — when the work counts as finished.

Claude Code, Cursor, Copilot in agent mode — these are **agent harnesses**, agentic environments.

When someone says "I work in Claude Code," it's not just about the model: the model is one component, and around it is a harness that provides the tools and runs the work loop.

**What's missing:** there is a workplace, but a generalist with no specific role is sitting at it. Nobody has set it up for a particular job.

## Concept 3. Agent — a model that can act

An **agent** is an LLM that the harness has allowed to choose its next actions itself, use tools, look at the result, and keep working in a loop until the goal is reached.

We additionally specialize the agent for a specific job:

> **agent = LLM (thinks) + harness (acts) + role (defines the job)**

An architecture reviewer, a test generator, a ticket triager — the same pattern with different roles.

The role answers the questions:

* what the agent does;
* what context it should read;
* which tools it may use;
* which rules it must follow;
* what a good result looks like.

No magic: a model, an execution environment, and a well-described job.

The role must be written down, not live in the author's head or in some random chat. Where exactly — we'll get there in concept 6.

**What's missing:** a role doesn't guarantee a result. Quality depends on **what the agent knows at the moment of action** — and it knows whatever made it into its current context.

## Concept 4. Context — everything the agent sees at the moment of work

**Context** is all the information available to the model at a given moment: the task, the instructions, the files provided, the project rules, the results of previous steps, tool outputs, and part of the working history.

> **The main law of practice: for the same model, context is the main controllable lever of quality.**
>
> The same agent will produce an excellent result or garbage depending on what you put into its context.

This is the most underrated skill. Beginners improve the prompt. Practitioners improve the **context**:

* which files to show;
* which rules to write down;
* which examples to provide;
* the results of which previous steps to keep;
* and what, on the contrary, **not** to show.

Excess context is harmful too: noise, money, and an extra chance the model pays attention to the wrong thing. This discipline is called **context engineering**.

The physical limit is the **context window**: how many **tokens** — small fragments of text — the model takes into account within a single call. Tokens are also the unit in which the cost of the model's work is calculated; the economics gets its own doc in this series.

That's why context is **managed**, not dumped in all at once.

**What's missing:** assembling the right context for every task by hand is expensive and unrepeatable. You want to save and reuse "how we do tasks of this type."

## Concept 5. Skill — a reusable step of work

A **skill** is a package of instructions and resources, written down in files, for a certain class of work; the harness loads it when the skill is needed. The definition is broad and matches the open Agent Skills standard: a skill may contain instructions, templates, reference materials, even executable scripts — and may describe a multi-step workflow.

But a standard is a language, not a discipline. Same as with Clean Code: the language lets you write in many ways, and the team picks its own rules and sticks to them. The convention of our factory (what a factory is — that's the whole [doc 03](03-ai-factory.md)):

> **One skill — one atomic step of work: the unit of work = the unit of testing.**
>
> If a step can't be verified separately from the others — keep splitting until it can. A sequence of steps is assembled into a chain — a **skill chain**.

The size test: "extract acceptance criteria from a ticket" (acceptance criteria — the list of checks by which a task counts as done) — that's a skill. But "do a code review" is, by our convention, already a chain:

```text
collect the diff
    ↓
check security
    ↓
check project conventions
    ↓
compose the findings
```

Each step is tested separately.

How do you even "test" a text file with instructions? Like a function: you feed it a known input and check the result against a predefined list of criteria (a rubric) or a reference answer. For example: take five real tickets with already-known correct acceptance criteria, run the skill several times, and see how consistently it produces what's needed.

The difference from a good **chat prompt**: a prompt is a one-off instruction inside a single conversation. A skill lives in files, is versioned, and is reused. Six months later the team has a library of proven ways of working, and similar tasks no longer start from zero.

Hence the second principle of our convention — teams keep getting it backwards:

> **The skill is primary, the agent is secondary.**
>
> The agent is a coordinator that runs the right skills in the right order and holds the context between them.

A practical smell (a warning sign — like a code smell): agents multiply, but the skill library doesn't grow. Worth checking whether the agents have turned into **"fat prompts"** — large, poorly reusable, and hard to test.

**What's missing:** all of this — the role, the context, the skills, the requirements for the result — has to be **written down** somewhere so that an agent can use it and a human can verify it, change it, and pass it on.

## Concept 6. Spec — a written definition of "what must be done"

A **specification (spec)** is a written document: what we're doing, what the constraints are, what counts as done. A spec can describe a specific task, an agent — that very "role" from concept 3, finally written down as a file — or an entire process.

> **A spec turns intent into an explicit instruction that an agent can use and a team can verify, discuss, and hand over.**

And from here comes the main shift in thinking that all agentic development stands on:

> **A file is memory. A chat is not.**

The result of work is a file in the repository: a spec, a skill, a report, a decision record (more on that below), code. Not a successful dialogue with a model. The chat history may physically survive, but it's an unreliable source of state: it may not be passed into context, may get truncated, lost among hundreds of messages, or simply never opened.

A file, on the other hand:

```text
gets committed
    ↓
gets reviewed
    ↓
gets versioned
    ↓
gets handed to another person or agent
```

The approach where the spec is the primary artifact and the agent uses it as its instruction is called **spec-driven development (SDD)**.

## Assembly

```text
LLM                     thinks, but doesn't act on its own
 └─ + harness           tools, a loop, and working state appear
     └─ + role          specialization for a job = AGENT
         └─ + context   knows what's needed at the moment of action
             └─ + skill reuses proven ways of working
                 └─ all of it is captured in files and specs
```

Unwinding the chain backwards:

```text
spec       says WHAT is required
skill      knows HOW to perform an individual step
context    holds what needs to be known right now
agent      makes the decisions
harness    makes those decisions executable
LLM        is the intelligence inside the loop
```

## Four more things you'll meet in real systems

* **Critic** — a checking step or a separate agent: it verifies the result of another step against the spec. It sits between stages and provides a "second look" before any human does:

  ```text
  plan
    ↓
  critic
    ↓
  implementation
    ↓
  critic
  ```

* **Orchestrator agent** — a dispatcher: it classifies the input (bug, feature, question, incident), picks the process, and hands work off between agents and skills. Not to be confused with the human orchestrator from [doc 01](01-economics-and-engineer-role.md): there it's an engineer's role, here it's a software component.

* **Hook** — a rule moved out of the prompt into executable code. A prompt **asks** ("don't commit without tests"); a hook **guarantees programmatically**:

  ```text
  commit attempted
       ↓
  tests run automatically
       ↓
  tests fail → commit is blocked
  ```

  A model can forget an instruction. A hook cannot.

* **Decision record** — a record not only of **what** was decided, but **why**. Without the "why," six months later somebody will see a strange decision, "improve" it — and step right back onto the problem it was created to avoid.

And one more requirement — **evidence**: reproducible proof of the work done (test results, check logs, reports, benchmark results). It is saved as artifacts, not left inside a conversation with a model. To the question "did you test this?" the answer "the agent said in the chat that everything works" is a bad one. A good one: "yes, here are the test results."

## The analogy in full: a new employee

Imagine we've hired a very smart new employee.

* **LLM** — their brain: knows a lot, reads fast, can reason and write code.
* **Harness** — their workplace: a laptop, access rights, a terminal, GitHub, a browser, and the rule "keep going until the task is done."
* **Agent** — the employee who has been given a specific role and the right to work independently.
* **Spec** — a written description of what is required of them and what counts as a good result.
* **Context** — the materials on their desk. Give them the wrong ones — they'll confidently do the wrong thing.
* **Skill** — the company playbook for "how we do X here," proven on dozens of tasks.
* **Files instead of chat** — knowledge that stays in the company and reaches the next person or agent.

A good AI team is not "we have a smart model": access to strong models is available to everyone today, and by itself it stops being an advantage. A good AI team is:

> **"We have the smart model's work well organized: specs are written, skills are accumulated, context is carefully selected, tools are set up, and the results are preserved."**

That is what an AI factory is gradually built from.


## Off-topic from Varabei

In the 1990s, when I was a teenager living in Minsk, the father of my childhood friends was a scientist. His name is Nikolai Zhigadlo.
Back then (as now :D) it was hard for me to even understand what exactly he did.
All I remembered: something about growing crystals and superconductors.
But what amazed me most at the time wasn't even the physics. In the 90s he was able to travel and work in the UK, Japan, Germany, and Switzerland — and his kids had real Pokémon cards and the first Game Boy.
For a teenager from 1990s Minsk, all of that looked like something from another universe.
In August 2026 I met him again. He's retired now.

In conversation he mentioned that he sometimes goes through his old experiment reports. And he does it together with plain ChatGPT. GPT understands the context of the experiments well and can explain individual processes. If 30 years ago it wasn't always clear why an experiment went one way and not another, now the model can sometimes offer an explanation that, to the scientist's eye, looks very plausible.

And then we talked about living in such an astonishing world that now, for $20 a month (and sometimes for free), without leaving home, you can discuss with an LLM the process of growing superconducting crystals at pressures of tens of thousands of atmospheres and temperatures around fifteen hundred degrees. The model understands solid-state physics, thermodynamics, ion diffusion, and so on.

When I got home, I gave GPT-5.6 Sol roughly this simple request myself:

"Explain in simple words: what iron-based superconducting crystals are, why physicists grow them, what they have to do with superconductivity, what that liquid and those ions are, what a flux is, why the position of the crystal inside the furnace matters, and so on."

And, unsurprisingly, I got a calm, detailed answer in simple words — roughly the kind Richard Feynman would probably have given.
The main thing was to correctly hand the model the context of my not-understanding :D
