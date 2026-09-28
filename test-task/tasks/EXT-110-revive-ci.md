# EXT-110 — Оживить CI на GitHub Actions

> Ядро курса · после BEVN-001 · ~2–4 часа · читать: [Инженерная база](../../ru/basics/engineering.md)

**Зачем.** После этого задания каждый PR проверяется машиной: сборка и тесты идут без вашей памяти и без мнения агента. Вы разбираете чужой пайплайн, переносите только то, что приносит пользу, и убеждаетесь, что CI действительно ловит ошибку, а не просто горит зелёным. С этого момента PR мержится только с зелёным CI ([правила](../rules.ru.md)).

## Тикет

The repository contains `.gitlab-ci.yml` — a CI pipeline from the platform the project used to live on. On GitHub it is dead weight: GitHub never executes it, so pull requests get no builds and no test runs. Figure out what the old pipeline did, and bring CI back to life on GitHub Actions. The docker-publish jobs are not needed (there is no registry to push to) — port only what earns its keep.

**Definition of Done**
- [ ] PR description summarizes what the old GitLab pipeline did, job by job, and what was ported vs dropped (and why)
- [ ] `.github/workflows/ci.yml` exists and runs on every pull request and on pushes to `main`
- [ ] Backend job: restore, build, and run unit tests
- [ ] Frontend job: install and build
- [ ] The workflow is green on this task's own PR
- [ ] `.gitlab-ci.yml` is removed — dead config confuses the next reader

## Докажи, что работает

- Ссылка на зелёный прогон CI на вашем PR.
- CI ловит ошибку: один раз намеренно сломайте тест в ветке задания, покажите красный прогон, потом почините. Оба коммита остаются в истории, ссылка на красный прогон — в PR.

## Объясни

- Что делала каждая job старого пайплайна и почему вы перенесли или выбросили каждую?
- Какие тесты на самом деле запускает ваш CI — и что может сломаться в проекте, а CI останется зелёным?
- Если зелёный CI — гейт для мержа, что обязательно должно быть в нём, чтобы гейт что-то значил?
- Что в этой задаче могло быть скриптом, а не работой агента?

## Цена

Стоимость задачи — одной строкой в devlog ([отчёт аналитики](../codemie-analytics.md) или `/usage` в Claude Code).
