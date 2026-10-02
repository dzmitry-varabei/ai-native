# BEVN-101 — Страница конференции тормозит

> Задание · ~0,5 дня · читать: [Инженерная база](../../ru/basics/engineering.md)

**Зачем.** Медленный запрос к базе — частый класс багов: ошибок нет, данные правильные, но страница грузится всё дольше по мере роста данных. После задачи вы умеете найти такой баг по числу SQL-запросов, исправить его и закрепить тестом, который без фикса падает.

## Тикет

Users are complaining that opening a conference page takes noticeably longer when the conference has many sessions. No errors, the data loads — it's just slow. Find out why and fix it. The data returned must stay the same.

**Definition of Done**
- [ ] Root cause identified and documented in a code comment at the fix location
- [ ] The sessions endpoint is fixed; other service methods with the same problem are listed in the PR description
- [ ] The number of SQL queries executed for the sessions endpoint is bounded regardless of session count
- [ ] No existing endpoint returns different data than before
- [ ] Covered by a test that fails without the fix

## Докажи, что работает

- Число SQL-запросов для эндпоинта сессий при одной сессии и при многих — до фикса и после.
- Тест, который падает без фикса и проходит с ним. Покажите красный прогон без фикса.

## Объясни

- Что именно происходило в базе и почему это не видно при малом числе сессий?
- Что ваш тест НЕ проверяет? Приведите пример неправильного фикса, который он пропустил бы.
- Что в этой задаче могло быть скриптом, а не работой агента?

## Цена

Стоимость задачи — одной строкой в devlog ([отчёт аналитики](../codemie-analytics.md) или `/usage` в Claude Code).
