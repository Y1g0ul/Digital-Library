---
created-dt: 2026-08-03 01:48
tags:
  - review
---
``` yaml
on:
  workflow_dispatch:
    inputs:

      # Строка
      name:
        description: "Имя"
        required: true
        default: "Nikita"
        type: string

      # Выбор из вариантов
      environment:
        description: "Окружение"
        required: true
        type: choice
        options:
          - dev
          - staging
          - production

      # Флажок
      deploy:
        description: "Выполнить deploy?"
        required: false
        default: false
        type: boolean
```

Получить значение input:
``` yml
${{ inputs.name }}
```

