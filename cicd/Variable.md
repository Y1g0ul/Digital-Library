---
created-dt: 2026-08-04 10:35
tags:
  - review
---
Значение, используемое для хранения настроек и данных [[Workflow]].

**`Типы`**
env
→ переменные окружения, заданные в Workflow.
`${{ env.MY_VARIABLE }}`

vars
→ configuration variables, заданные на уровне:
  Repository / Organization / Environment.
`${{ vars.MY_VARIABLE }}`

github
→ встроенные переменные с информацией о Workflow и событии.
`${{ github.repository }}`

Configuration Variable:
- только буквы, цифры и `_`;
- не может начинаться с `GITHUB_`;
- не может начинаться с цифры;
- имена не чувствительны к регистру;
- максимум 48 KB на одну Variable.

Переменные `GITHUB_*` и `RUNNER_*` нельзя переопределять.

**`Приоритет vars`**

Environment
  ↓
Repository
  ↓
Organization

Более низкий уровень имеет приоритет.
