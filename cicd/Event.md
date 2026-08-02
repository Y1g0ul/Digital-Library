---
created-dt: 2026-07-28 11:52
tags:
  - review
sr-due: 2026-08-05
sr-interval: 5
sr-ease: 241
---
Cобытие, которое запускает выполнение Pipeline (Workflow).

Примеры Event
- `push` - отправка изменений в репозиторий.
- `pull_request` - создание или обновление [[pull]] Request.
- `schedule` - запуск по расписанию ([[cron]]).
- `workflow_dispatch` - ручной запуск.
- `release` - создание релиза.
- `tag` - создание тега.

Разработчик выполняет:
git [[push]] origin main
↓
Срабатывает Event `push`
↓
Запускается [[Workflow]]
↓
Выполняется [[Pipeline]]


