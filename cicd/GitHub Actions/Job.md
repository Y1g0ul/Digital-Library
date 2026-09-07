---
created-dt: 2026-07-28 12:05
tags:
  - review
sr-due: 2026-10-24
sr-interval: 54
sr-ease: 250
---
Отдельная задача (этап работы) внутри [[cicd/GitHub Actions/Pipeline]]. Каждый Job выполняет определенную цель и состоит из одного или нескольких [[Step]]s. Может:
- выполняться последовательно или параллельно с другими Job;
- зависеть от выполнения других Job;
- запускаться на Runner.

Позволяет разделить [[cicd/GitHub Actions/Pipeline]] на независимые этапы.
- сборка приложения;
- запуск тестов;
- публикация [[docker]]-образа;
- деплой.

[[cicd/GitHub Actions/Pipeline]]
├── Build Job
├── Test Job
└── Deploy Job
Каждый Job содержит свои [[Step]]s.

