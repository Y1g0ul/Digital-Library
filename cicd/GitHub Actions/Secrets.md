---
created-dt: 2026-08-06 13:57
tags:
  - review
sr-due: 2026-09-09
sr-interval: 19
sr-ease: 250
---
Конфиденциальное значение, которое GitHub [[Action]]s
хранит отдельно от кода.

Используется для:
- паролей;
- токенов;
- API keys;
- [[networks/SSH|SSH]] keys.

Получить Secret:
``` yml
${{ secrets.MY_SECRET }}
```

Например:
```yaml
- run: echo "${{ secrets.API_TOKEN }}"
```
