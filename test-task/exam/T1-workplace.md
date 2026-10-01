# T1 — Рабочее место

> Тестовое · первое задание · ~3 ч · читать: [контекст агента](../../ru/basics/context.md), [хуки](../../ru/basics/hooks.md)

**Зачем.** Прежде чем давать агенту задачи, настройте, что он видит и чего не видит. После этой задачи вы подключаете к агенту внешний инструмент через MCP, закрываете ему секреты программно, а не просьбой в тексте, и знаете, что на самом деле уходит в модель. Закрывает провал «работаю с агентом как с чатом» и реальный случай, когда агент прочитал конфиг MCP и напечатал токен в чат.

## Тикет

Set up the working environment for the coding agent in your copy of the [code template](https://github.com/dzmitry-varabei/brown-events-pilot).

1. Connect the GitHub MCP server to your coding agent. Then ask the agent to create one GitHub issue per exam task (T1, T2, T3) in your repository. Do not create issues by hand in the web UI.
2. The MCP server needs a personal access token. Keep it out of the agent's session: keep the MCP config outside the repository or reference an environment variable instead of the literal token. Then block the agent from reading `.env` and the MCP config — with a `deny` rule in the permission settings and/or a `PreToolUse` hook. A line in `CLAUDE.md` like "do not read .env" is not enough.
3. Start a new session, type "hi", and find out what is actually sent to the model. In Claude Code run `/context` (required). Optionally, put a logging proxy between the agent and the API (see the exercise in [before the interview](../../ru/interview/before-interview.md) — note that a custom base URL turns off some optimizations, so the request gets bigger).

**Definition of Done**
- [ ] GitHub MCP server configured; how you did it — a couple of lines in `docs/devlog.md`
- [ ] Issues for T1, T2, T3 exist in your repository, title `<ID> — <task name>`, body contains the full task text; created by the agent through MCP
- [ ] The token is not in the repository and not in any file the agent can read
- [ ] Reading `.env` and the MCP config is blocked by a `deny` rule and/or a hook, committed in `.claude/settings.json` (or your agent's equivalent) — including through shell commands such as `cat .env`
- [ ] The devlog lists the three heaviest parts of the request for "hi" and their approximate size in tokens
- [ ] Every later PR references its issue (`Closes #N`)

## Докажи, что работает

- Вывод или скриншот в PR: создайте `.env` с выдуманным значением (не настоящим токеном), попросите агента его прочитать — вызов заблокирован правилом или хуком. Проверьте и обход через `cat .env` в `Bash`.
- Запрет чтения и хук снижают риск случайной утечки, но не изолируют секрет полностью: например, переменные окружения процесса агента он может увидеть.
- Ссылка на issues в devlog и строка из истории сессии, где агент вызывает инструмент GitHub MCP для их создания.
- Вывод `/context` на «привет» в devlog (без секретов и ключей).

Если токен всё же засветился в сессии — перевыпустите его и напишите об этом в devlog. Это не провал, провал — промолчать.

## Объясни

- «Claude Code отправляет в модель всю кодовую базу» — почему это неправда, и с какой оговоркой это правда? Опирайтесь на свой вывод `/context`.
- Почему запрет в `deny` или хук надёжнее, чем правило «не читай .env» в `CLAUDE.md`? Что будет, если агент всё же прочитает файл?
- Какая из трёх тяжёлых частей запроса вас удивила и что вы можете с ней сделать?
- Что в этой задаче могло быть скриптом, а не работой агента?

## Цена

Стоимость задачи — в devlog: [отчёт аналитики](../codemie-analytics.md) или `/usage` в Claude Code.
