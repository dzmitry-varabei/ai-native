# Interview questions: the AI part

> Written in tandem: Dzmitry Varabei and Claude Fable, September 21, 2026.
> [Читать по-русски](../ru/interview/ai-part.md). Translated from the Russian original; the Russian version is canonical.

Questions that come up at interviews for AI Native positions, the AI part. There are no answers here: next to each question, in brackets, is the area from the [AI SDLC requirements](./requirements/ai-sdlc.md) to prepare. Questions on SDLC and critical thinking are in the [SDLC requirements](./requirements/sdlc.md).

## Already asked in interviews

- How would you onboard an agent into a project? (context engineering; agent tooling)
- Why were hooks invented? (guardrails and rule automation)
- What context does the agent get when a session starts? (working with a coding agent; context engineering)

## CLAUDE.md / AGENTS.md

Area: context engineering.

- What is CLAUDE.md (AGENTS.md) and what should be in it?
- What should not be in it, and why?
- How do you check that the agent has really read the file and follows it?
- The agent ignores a rule from CLAUDE.md. What do you do?
- What is the difference between CLAUDE.md and AGENTS.md? When do you need one, and when both?
- Where do these files live (global, project, folder), and how do they combine?
- How do you keep the file up to date when the project changes?

## Hooks in Claude Code

Area: guardrails and rule automation.

- What events do hooks in Claude Code have? What would you attach to them?
- Give an example of a hook that blocks a bad commit. What exactly does it check?

## What goes to the API

Area: working with a coding agent; context engineering; cost awareness.

- You wrote only "hi" to the agent. What goes to the Anthropic API request besides that word?
- How can you see it? How would you debug the exchange between Claude Code and the API?

## Practice before the interview

Area: cost awareness (tokenomics).

Do the same task several times in different ways: Opus + Haiku, pure Sonnet, Sonnet + superpowers. Compare the cost in dollars.

At the interview: what did you get, where does the difference in price and quality come from, and which option would you choose on a project with a monthly AI budget?
