# EXT-120 — Новая регистрация не появляется в списке

> Задание · ~0,5 дня · читать: [Инженерная база](../../ru/basics/engineering.md)

**Зачем.** Устаревшее состояние на фронте после изменения данных — частый класс багов: запрос прошёл, данные в базе есть, а экран показывает старое. После задачи вы воспроизводите такой баг до фикса и закрепляете фикс тестом, который без фикса падает.

## Тикет

On a conference page (`/conferences/:id`), I register through the "Register Now" modal and get a success message. But the Registrations section on the same page does not show the new attendee. The attendee appears only after I reload the page.

**Definition of Done**
- [ ] After a successful registration, the Registrations section shows the new attendee without a page reload
- [ ] Existing registrations stay in the list
- [ ] A failed registration does not change the list
- [ ] Covered by a test that fails without the fix

## Докажи, что работает

- Тест, который падает без фикса и проходит с ним: компонентный на Vitest или e2e на Playwright, на выбор. Покажите красный прогон без фикса (коммит с тестом до фикса или вывод команды в PR).
- Шаги воспроизведения бага в описании PR, записанные до фикса.

## Объясни

- Где живёт список регистраций и почему он не узнаёт о новой записи?
- Какие варианты фикса вы видели и почему выбрали свой?
- Что в этой задаче могло быть скриптом, а не работой агента?

## Цена

Стоимость задачи — одной строкой в devlog ([отчёт аналитики](../codemie-analytics.md) или `/usage` в Claude Code).
