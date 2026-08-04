---
created-dt: 2026-08-04 14:58
tags:
  - review
---
Отдельное окружение [[Workflow]]. У каждого Environment могут быть свои [[Variable]]s, Secrets. У GitHub фактически есть несколько уровней хранения:
``` 
Repository
├── Variables
├── Secrets
│
└── Environments
    ├── Variables
    └── Secrets
```

