# T3 — Один баг тремя способами

> Тестовое · после T2 · ~6 ч · читать: [что такое фабрика](../../ru/basics/factory.md#когда-фабрика-не-нужна), [куда уходят токены](../../ru/basics/tokens.md)

**Зачем.** Вы чините один баг тремя способами и сравниваете их по цене, времени и качеству на своих цифрах, а не по чужим обещаниям. После задачи вы умеете воспроизвести баг до фикса, закрепить его тестом, который падает без фикса, и выбрать уровень процесса под задачу. Закрывает провалы «сделал для галочки, не проверил, что работает» и «не умею дебажить».

## Тикет

Pick one bug by your stack:

- **Frontend** — [BEVN-115](../tasks/BEVN-115-registration-modal-state.md): the registration modal keeps stale form data and the success screen after it is closed.
- **Backend** — [BEVN-101](../tasks/BEVN-101-sessions-page-slow.md): the conference page gets slower with more sessions because of N+1 SQL queries.

Reproduce the bug first, before any fix. Then fix it three times, in three branches from the same commit:

- **(a)** Sonnet, no process: describe the bug, let the agent fix it.
- **(b)** Sonnet + [superpowers](https://github.com/obra/superpowers): spec → plan → code. In another agent — the same flow with `spec.md` and `plan.md`.
- **(c)** Opus plans, Haiku executes.

In another agent, use its equivalents: a mid-tier model for (a) and (b), a strong model planning and a cheap one executing for (c). Name the models in the table.

Merge only the best branch through a PR. Keep the other two branches pushed for review.

**Definition of Done**
- [ ] Steps to reproduce the bug are in the devlog, written before any fix
- [ ] Each branch has a test that fails without the fix and passes with it. Frontend: a component test or a Playwright e2e test. Backend: an integration test that checks the number of SQL queries for the endpoint does not grow with the number of sessions
- [ ] Each branch is taken as far as that approach gets; where it falls short of the ticket's Definition of Done, the table says what is missing
- [ ] The Playwright smoke test from the code repository runs in each branch; output attached
- [ ] The devlog has a table: approach → cost → time → quality (what it missed or had to redo)
- [ ] One branch merged via PR; the other two stay on the remote

Про бэкенд-тест: существующие тесты работают на EF Core InMemory, а он не выполняет SQL, так что считать запросы на нём нечем. Нужен реляционный провайдер: PostgreSQL из `docker-compose` или SQLite.

Про smoke-тест: он появляется в шаблоне [brown-events-pilot](https://github.com/dzmitry-varabei/brown-events-pilot). Если в вашей копии его ещё нет — пропустите этот пункт и напишите об этом в devlog.

## Докажи, что работает

- Вывод теста в каждой ветке: красный без фикса, зелёный с фиксом. Для бэка — число SQL-запросов при 1 и при N сессиях.
- Вывод Playwright smoke-теста из каждой ветки (если тест есть в шаблоне).
- В devlog под таблицей — вывод: какой путь вы бы взяли на проекте и почему; в какой момент полный процесс окупается, а в какой нет.

## Объясни

- Почему тест именно этого уровня пирамиды? Для бэка: почему e2e-тест не поймал бы N+1?
- Откуда разница в цене между способами? Покажите по отчёту, на каком шаге ушли токены.
- Что пропустил или сделал иначе самый дешёвый способ — и заметили бы вы это без теста?
- Что в этой задаче могло быть скриптом, а не работой агента?

## Цена

Стоимость каждой ветки — в таблице в devlog: [отчёт аналитики](../codemie-analytics.md) или `/usage` в Claude Code.
