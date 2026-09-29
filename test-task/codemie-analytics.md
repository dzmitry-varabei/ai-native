# CodeMie Analytics for Students
> Written in tandem: Dzmitry Varabei and Claude Fable, critiqued by GPT Sol.
> August 24, 2026.

How to build a report on your work with AI agents in one command — and send the reviewer only what belongs to the test task.

## What This Tool Is

[CodeMie CLI](https://github.com/codemie-ai/codemie-code) (`@codemieai/code`) is an open-source utility (Apache-2.0) from the EPAM AI/Run team. It's a fairly large toolbox: a single launcher for different AI agents, corporate and external model access, SSO and proxy, agent installation and configuration, analytics, and more. Some capabilities target corporate use and may require an EPAM account or infrastructure. You won't need any of that — out of everything CodeMie CLI can do, we only need the analytics, available through the `codemie analytics` command.

**The `codemie analytics` command** reads the local session logs that your AI agents already write to disk (Claude Code — to `~/.claude`; likewise Codex, Gemini CLI, Copilot CLI, Pi) and assembles them into a readable report: how many sessions, how many turns and tool calls, which files changed, how many tokens were spent. You keep working in your usual tool — CodeMie doesn't intercept anything and doesn't sit "between" you and the agent.

> **On privacy.** The `codemie analytics` report is just two files (`.html` and `.json`) created **locally in the current folder**. The command sends nothing anywhere: not to EPAM, not to CodeMie servers, not to the reviewer. Only you see the report, and only the person you choose to send it to. Before sending, open the `.html` in a browser and check what's inside.
>
> What gets into the report: session metrics, project folder paths, and **the text of your first message in each session**. Full dialogues with the agent do not.

## What You'll Need

- **Node.js version 20 or newer**.
- **The console AI agent** you work in: Claude Code, Codex CLI, Gemini CLI, Copilot CLI, or Pi. *Cursor, web chats (claude.ai, ChatGPT), and IDE plugins don't make it into the report — the task must be done in a console agent.*

Installing CodeMie globally is not necessary — every command below runs through `npx` and downloads the utility on the fly.

## See the Analytics for All Your Projects

This is an optional "for yourself" step — to see what the report looks like and get the stats of all your agent work for a week:

```bash
npx -y -p @codemieai/code codemie analytics \
  --last 7d --include-external \
  --export both -o ./my-report/
```

`codemie-analytics-<date>.html` (a dashboard — open it in a browser) and `codemie-analytics-<date>.report.json` (the same data for machine processing) appear in the `my-report/` folder. The `--include-external` flag includes all native agent sessions on the machine; `--last 7d` is a 7-day window (you can use `--last 24h`, or exact dates via `--from 2026-08-20 --to 2026-08-24`).

*Don't send this full report to anyone — it contains all your projects and personal sessions.*

## Build the Report for the Test Task

1. **Work in a dedicated folder** — your working repository copy — and only in it.

2. **Do the task with your agent**, launching it from that folder — e.g. `claude`, `codex`, or `gemini`. Work as you normally would: the number of attempts and clarifications is not penalized; the overall approach is what's assessed. Don't wait for the end: after your first task, run the command from step 3 once — you will see what one task cost. Watching the price from the first task is what the analytics is for you, not only for the reviewer.

3. **When done, build the report for that folder only** (you can run it from the folder itself). The report must cover **the whole period of the task** — usually 14–20 days, hence the 30-day window in the command:

   ```bash
   npx -y -p @codemieai/code codemie analytics \
     --last 30d --include-external \
     --project brown-events-pilot \
     --export both -o ./report/
   ```

   *`--project brown-events-pilot` filters by folder name: only sessions from it get into the report; the rest of your projects and personal sessions stay out. If your folder is named differently — substitute your name. If the task took longer than a month — give exact dates: `--from ... --to ...`.*

4. **Check and send.** Open `report/codemie-analytics-<date>.html` in a browser and make sure it contains only the test-task sessions. Then deliver both files — `codemie-analytics-<date>.report.json` and `codemie-analytics-<date>.html` — the way the task rules ask (committed to your repository).

## Important! The Task Number Goes into the Branch, the Commits, and the Sessions

Every activity must be tied to its task number — that's how the reviewer traces the whole chain "ticket → agent sessions → commits → PR". In commits and PRs this is routine; here is how the number gets **into the sessions**: the analytics report records two fields per session — the git branch and the text of your first message. The number is threaded through them:

1. **Branch = task number.** Before starting a task, create a branch with its number: `git checkout -b BEVN-104-standardize-responses`. Every agent session on that branch automatically gets the number in the report's `branch` field — the most reliable mechanism, impossible to forget.
2. **Start the session's first message with the number:** "*BEVN-104: standardize the API error responses...*". The first message becomes the session's title — the session list reads as "which session — for which task". This is the backup for sessions where no branch exists yet (discussion, planning on `main`).
3. **Commits and PRs** — as the rules ask: the number in the commit message (`feat: add waitlist (BEVN-202)`) and in the PR title.

Bonus: the report can be sliced per task by branch — `codemie analytics --branch BEVN-202 --export html ...` shows only that task's sessions.

## What the Reviewer Will See

| In the report | Not in the report |
|---|---|
| Number of sessions, turns and tool calls, success share | Your full dialogues with the agent |
| Each session's first message — how you framed the task | The contents of your files |
| Which files changed, lines added/removed, languages | Your keys, tokens, and passwords |
| Models, tokens, and a cost estimate | Anything from other folders — when `--project` is used |

The metrics are not there to "catch" you on the number of attempts, but to see *how* you work with the agent: how you frame the task, how you iterate, whether you drive it to a result. This is assessed together with the solution itself.
