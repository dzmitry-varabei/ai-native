# BEVN-115 — Модалка регистрации показывает старые данные

> Задание · ~0,5 дня · читать: [Инженерная база](../../ru/basics/engineering.md)

**Зачем.** Это задание про дебаг, а не про фикс. После него вы воспроизводите баг до правки, формулируете гипотезу и проверяете её, прежде чем что-то менять, и закрепляете фикс тестом, который без фикса падает. Закрывает частый провал: агент «чинит» наугад, баг вроде бы уходит, но никто не знает почему и вернётся ли он.

## Тикет

When a user opens the registration modal, partially fills in the form, and then closes it, the fields still contain the old data the next time the modal is opened. After a successful registration, reopening the modal shows the success screen instead of a fresh form — making it impossible to start a new registration without refreshing the page.

**Definition of Done**
- [ ] Closing the modal resets all form fields to empty
- [ ] Closing the modal clears any validation errors and server error messages
- [ ] After a successful registration, reopening the modal presents a fresh empty form
- [ ] Multiple open/close cycles do not accumulate state

## Докажи, что работает

- Шаги дебага в описании PR: как воспроизвели баг до фикса → гипотеза → как её проверили → что подтвердилось или опроверглось.
- Тест, который падает без фикса и проходит с ним: компонентный или e2e, на выбор. Во фронтенде тестов пока нет — настройку тестового инструмента делаете в этом же PR. Покажите красный прогон без фикса (коммит с тестом до фикса или вывод команды в PR).

## Объясни

- Где жило состояние формы и почему оно переживало закрытие модалки?
- Какую гипотезу вы проверили первой и чем? Если первая не подтвердилась — что навело на следующую?
- Почему ваш тест упал бы, если кто-то вернёт баг? Что он не ловит?
- Что в этой задаче могло быть скриптом, а не работой агента?

## Цена

Стоимость задачи — одной строкой в devlog ([отчёт аналитики](../codemie-analytics.md) или `/usage` в Claude Code).
