# EXT-100 — Импорт маршрута в GitHub Issues через MCP

> Ядро курса · старт · ~1–2 часа · читать: [Контекст агента](../../ru/basics/context.md)

**Зачем.** Вы подключаете агенту внешний инструмент через MCP (протокол, которым к агенту подключают трекер, базу, документацию) и получаете трекер, на который ссылается каждая ветка и каждый PR: «тикет → ветка → PR», как на реальном проекте. Заодно учитесь держать секрет вне контекста агента: токен, который агент однажды прочитал, остаётся в логе сессии на диске, и его может прочитать любой другой инструмент на вашей машине.

## Тикет

The route lives as markdown files in this program repository. Your **working repository** (your copy of the [code template](https://github.com/dzmitry-varabei/brown-events-pilot)) needs its own tracker: one GitHub Issue per task, so that every branch and PR can reference the issue it implements.

Don't create the issues by hand. Set up the **GitHub MCP server** for your coding agent and have the agent create the issues. Import the course core tasks in route order: EXT-100, BEVN-001, EXT-110, BEVN-115, BEVN-202 (see [course](../course.ru.md)). Add module tasks as issues when you take them.

The GitHub MCP server needs a personal access token. Keep the token out of the agent's session: keep the MCP config outside the repository, or reference an environment variable instead of the literal token. If the token was ever printed in the chat, revoke it and create a new one.

**Definition of Done**
- [ ] GitHub MCP server configured for your coding agent (how you did it — a couple of lines in `docs/devlog.md`)
- [ ] Your repository has one issue per core task, in route order
- [ ] Each issue: title `<ID> — <task name>`, body contains the full task text copied from this repository
- [ ] The issues were created by the agent through MCP — not by hand in the web UI
- [ ] The token is not in the repository, not in the chat, and not in any file the agent reads
- [ ] From this point on, every PR description references its issue (`Closes #N`)

## Докажи, что работает

- Ссылка на список issues в PR: пять заданий ядра в порядке маршрута, у каждого — полный текст задания.
- В PR — где лежит конфиг MCP и как в него попадает токен (переменная окружения или файл вне репозитория). Сам токен не показывайте.

## Объясни

- Агент консольный и пишет весь лог сессии на диск. Куда попадёт токен, если вставить его в чат? А если агент сам прочитает `mcp.json` с токеном внутри?
- Конфиг MCP не подхватил `.env`, и токен захардкодили в `mcp.json`, добавленный в `.gitignore`. Почему это не решает проблему? Что решает её надёжно? (Подсказка: хук, который запрещает агенту читать файлы с секретами, — [EXT-302](../side-quests/EXT-302-hook-not-reminder.md).)
- Что делать, если токен всё-таки напечатался в чате?
- Что в этой задаче могло быть скриптом, а не работой агента?

## Цена

Стоимость задачи — одной строкой в devlog ([отчёт аналитики](../codemie-analytics.md) или `/usage` в Claude Code).
