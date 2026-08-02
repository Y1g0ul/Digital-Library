---
created-dt: 2026-07-29 12:25
tags:
  - review
sr-due: 2026-08-05
sr-interval: 3
sr-ease: 250
---
Готовый переиспользуемый блок, который выполняет определенную задачу внутри [[Step]]. Вместо того чтобы писать команды вручную, можно использовать уже готовое [Action](https://github.com/marketplace?type=actions):
- получить код из репозитория;
- настроить окружение;
- установить язык программирования;
- войти в Docker Registry;
- загрузить артефакты;
- выполнить другие типовые задачи.

```yaml
steps:
  - name: Checkout repository
    uses: actions/checkout@v4

  - name: Setup Node.js
    uses: actions/setup-node@v4

  - name: Run tests
    run: npm test
```

Workflow
↓
Job
↓
Step
├── uses → Action
└── run → Команда

