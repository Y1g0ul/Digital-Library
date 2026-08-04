---
created-dt: 2026-08-04 14:58
tags:
  - review
---
Отдельное окружение [[Workflow]]. У GitHub фактически есть несколько уровней хранения:
``` 
Repository
├── Variables
├── Secrets
│
└── Environments
    ├── Variables
    └── Secrets
```

Они позволяют разделять настройки **по окружениям**, например:
- development
- staging
- production
У каждого Environment могут быть свои [[Variable]]s и Secrets. Причём Environment можно привязать к [[Job]]:
``` yml
jobs:
  deploy:
    environment: production
    runs-on: ubuntu-latest

    steps:
      - run: echo "${{ vars.APP_URL }}"
```
Тогда [[Job]] получает настройки именно из `production`.

