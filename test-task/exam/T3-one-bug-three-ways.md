# T3 — Один баг тремя способами

> Тестовое · после T2 · ~6 ч · читать: [что такое фабрика](../../ru/basics/factory.md#когда-фабрика-не-нужна), [куда уходят токены](../../ru/basics/tokens.md)

**Зачем.** Вы чините один баг тремя способами и сравниваете их по цене, времени и качеству на своих цифрах, а не по чужим обещаниям. После задачи вы умеете воспроизвести баг до фикса, закрепить его тестом, который падает без фикса, и выбрать уровень процесса под задачу. Закрывает провалы «сделал для галочки, не проверил, что работает» и «не умею дебажить».

## Тикет

Pick one bug by your stack:

- **Frontend** — [EXT-120](../tasks/EXT-120-registrations-list-stale.md): after a successful registration, the new attendee does not appear in the Registrations section until the page is reloaded.
- **Backend** — [BEVN-101](../tasks/BEVN-101-sessions-page-slow.md): the conference page gets slower with more sessions because of N+1 SQL queries.

Reproduce the bug first, before any fix. Then fix it three times, in three branches from the same commit:

- **(a)** Sonnet, no process: describe the bug, let the agent fix it.
- **(b)** Sonnet + [superpowers](https://github.com/obra/superpowers): spec → plan → code. In another agent — the same flow with `spec.md` and `plan.md`.
- **(c)** Opus plans, Haiku executes.

In another agent, use its equivalents: a mid-tier model for (a) and (b), a strong model planning and a cheap one executing for (c). Name the models in the table.

Merge only the best branch through a PR. Keep the other two branches pushed for review.

Keep the comparison fair: all three branches start from the same commit; each approach runs in new sessions, with no results of the other attempts in the context; every branch goes through the same set of acceptance checks; any manual intervention is recorded in the table. The conclusion is about this experiment, not a ranking of models.

**Definition of Done**
- [ ] Steps to reproduce the bug are in the devlog, written before any fix
- [ ] Each branch has a test that fails without the fix and passes with it. Frontend: a component test or a Playwright e2e test. Backend: an integration test that checks the number of SQL queries for the endpoint does not grow with the number of sessions
- [ ] Every branch keeps the attempt, the test and the actual result; the ticket's full Definition of Done is required only for the branch you merge. Where another branch falls short of it, the table says what is missing
- [ ] The Playwright smoke test from the code repository runs in each branch; output attached
- [ ] The devlog has a table: approach → cost → time → quality (what it missed or had to redo)
- [ ] One branch merged via PR; the other two stay on the remote

Про smoke-тест: он появляется в шаблоне [brown-events-pilot](https://github.com/dzmitry-varabei/brown-events-pilot). Если в вашей копии его ещё нет — пропустите этот пункт и напишите об этом в devlog.

## Докажи, что работает

- Вывод теста в каждой ветке: красный без фикса, зелёный с фиксом. Для бэка — число SQL-запросов при 1 и при N сессиях.
- Вывод Playwright smoke-теста из каждой ветки (если тест есть в шаблоне).
- В devlog под таблицей — вывод: какой путь вы бы взяли на проекте и почему; в какой момент полный процесс окупается, а в какой нет.

## Объясни

- Почему тест именно этого уровня пирамиды? Что измеряет ваш тест и почему обычная проверка интерфейса может пропустить этот баг?
- Что ваш тест НЕ проверяет? Приведите пример неправильного фикса, который он пропустил бы.
- Откуда разница в цене между способами? Покажите по отчёту, на каком шаге ушли токены.
- Что пропустил или сделал иначе самый дешёвый способ — и заметили бы вы это без теста?
- Что в этой задаче могло быть скриптом, а не работой агента?

## Цена

Стоимость каждой ветки — в таблице в devlog: [отчёт аналитики](../codemie-analytics.md) или `/usage` в Claude Code.
