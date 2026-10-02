# T2 — Карта и CLAUDE.md

> Тестовое · после T1 · ~5 ч · читать: [контекст агента](../../ru/basics/context.md), [инженерная база](../../ru/basics/engineering.md)

**Зачем.** После этой задачи вы можете рассказать проект — фронт, бэк, как они общаются, что лежит в базе — без открытия файлов, и одну фичу до уровня тестов. Это спросят на созвоне. Вторая часть учит превращать знание «для людей» в короткий файл, который агент читает в начале каждой сессии. Закрывает провал «агент написал карту, я её не проверил и не могу объяснить архитектуру».

## Тикет

**Part 1 — BEVN-001 Codebase Mapping.**

Explore the backend codebase and produce a written map of what exists. Document the project structure, the responsibility of each layer (Controllers, Services, Models, Data), and the relationships between entities. Draw an entity-relationship diagram. Identify the request flow from HTTP call to database and back. At the end, a new team member should be able to understand the architecture from your document without reading the code.

**Part 2 — Project knowledge file.**

Compress the map into a `CLAUDE.md` (or `AGENTS.md`) in the repository root, no more than 50 lines, written for the agent (imperative, no filler): how to run and test the project, conventions, the architecture in a few lines, known traps. Then check the effect: ask the same question about the project in a new session without the file and with it.

**Definition of Done**
- [ ] Entity-relationship diagram created (any format: draw.io, Mermaid, plain ASCII)
- [ ] Layer responsibility table written: Controllers, Services, Models/Data mapped to their roles
- [ ] Request flow documented for at least 3 endpoints end-to-end
- [ ] Saved as `docs/architecture.md` in the project root
- [ ] The frontend is on the map: main modules, how it calls the backend, what is stored in the database
- [ ] `CLAUDE.md` (or `AGENTS.md`) in the repository root, 50 lines or fewer
- [ ] The devlog has a before/after comparison: the same question, answer without the file and answer with it

## Докажи, что работает

- В devlog: вопрос про проект и два ответа агента — из новой сессии без `CLAUDE.md` и с ним. Коротко: что стало точнее, что осталось неверным.
- На созвоне: вы рассказываете регистрацию на конференцию от кнопки во фронте до строки в базе — что происходит на каждом слое и какими тестами это покрыто (или не покрыто). Без открытия файла.

## Объясни

- Что вы выкинули из карты при сжатии в `CLAUDE.md` и почему? Что агент и так найдёт в коде сам?
- Что в карте вы проверили по коду сами, а что взяли у агента на веру? Покажите одно утверждение карты, которое вы проверили по коду, и код, который его подтверждает; если что-то пришлось исправить — что именно.
- Что во втором ответе агент взял из `CLAUDE.md`, а что проверил в коде?
- Какие тесты сейчас покрывают регистрацию? Если никакие — какой её баг вы бы закрыли тестом первым и почему?
- Что в этой задаче могло быть скриптом, а не работой агента?

## Цена

Стоимость задачи — в devlog: [отчёт аналитики](../codemie-analytics.md) или `/usage` в Claude Code.
