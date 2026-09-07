---
created-dt: 2026-07-28 12:11
tags:
  - review
sr-due: 2026-10-25
sr-interval: 55
sr-ease: 250
---
Отдельное действие внутри [[cicd/GitHub Actions/Job]]. Step выполняет одну конкретную операцию: запускает команду, использует [[Action]] или выполняет скрипт. Разбивает [[cicd/GitHub Actions/Job]] на последовательность простых действий:
- Получить код из репозитория.
- Установить зависимости.
- Запустить тесты.
- Собрать [[docker]]-образ.
- Загрузить артефакты.

Build Job
├── Checkout repository
├── Install dependencies
├── Build application
└── Upload artifact
