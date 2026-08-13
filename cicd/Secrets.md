---
created-dt: 2026-08-06 13:57
tags:
  - review
sr-due: 2026-08-21
sr-interval: 8
sr-ease: 250
---
Конфиденциальное значение, которое GitHub Actions
хранит отдельно от кода.

Используется для:
- паролей;
- токенов;
- API keys;
- SSH keys.

Получить Secret:
``` yml
${{ secrets.MY_SECRET }}
```

Например:
```yaml
- run: echo "${{ secrets.API_TOKEN }}"
```
