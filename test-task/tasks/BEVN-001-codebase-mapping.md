# BEVN-001 — Карта кодовой базы

> Ядро курса · после EXT-100 · ~1 день · читать: [Инженерная база](../../ru/basics/engineering.md)

**Зачем.** После этого задания вы можете рассказать чужой проект целиком: фронтенд, бэкенд, как они общаются, что лежит в базе, — и провести одну фичу через все слои вплоть до тестов. Закрывает частый провал: агент пишет документ, документ никто не сверяет с кодом, и на созвоне человек не может объяснить архитектуру. Карта ваша, а не агента: всё, что в ней есть, вы можете объяснить.

## Тикет

Explore the backend codebase and produce a written map of what exists. Document the project structure, the responsibility of each layer (Controllers, Services, Models, Data), and the relationships between entities. Draw an entity-relationship diagram. Identify the request flow from HTTP call to database and back. At the end, a new team member should be able to understand the architecture from your document without reading the code.

**Definition of Done**
- [ ] Entity-relationship diagram created (any format: draw.io, Mermaid, plain ASCII)
- [ ] Layer responsibility table written: Controllers, Services, Models/Data mapped to their roles
- [ ] Request flow documented for at least 3 endpoints end-to-end
- [ ] Saved as `docs/architecture.md` in the project root
- [ ] The frontend is on the map too: main modules, how it calls the backend
- [ ] One feature (registration is a good choice) described end-to-end: what happens on each layer, and which tests cover it

## Докажи, что работает

- Карта сверена с кодом: в PR — список того, что вы поправили в тексте агента после сверки (неверные связи, выдуманные слои, пропущенные сущности). Если править было нечего — что именно вы проверили, чтобы так решить.
- Для каждого из трёх эндпоинтов в request flow — ссылка на файл и метод, где запрос реально проходит каждый слой.

## Объясни

- Представьте созвон с заказчиком: расскажите проект на верхнем уровне — фронт, бэк, как они общаются, база, — не открывая файл.
- Проведите регистрацию end-to-end: что происходит на каждом слое и какие тесты это покрывают. Чего тестами не покрыто? На проекте без тестировщиков это спросят первым.
- Что в карте агента оказалось неверным или слишком общим — и как вы это заметили?
- Что в этой задаче могло быть скриптом, а не работой агента?

## Цена

Стоимость задачи — одной строкой в devlog ([отчёт аналитики](../codemie-analytics.md) или `/usage` в Claude Code).
