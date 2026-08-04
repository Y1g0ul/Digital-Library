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

| Variable             | Что содержит                                                        |
| -------------------- | ------------------------------------------------------------------- |
| `GITHUB_ACTOR`       | Пользователь или приложение, запустившее [[Workflow]]               |
| `GITHUB_EVENT_NAME`  | Событие, которое запустило Workflow (`push`, `pull_request` и т.д.) |
| `GITHUB_REF`         | Полный ref ветки или тега, запустившего Workflow                    |
| `GITHUB_REF_NAME`    | Короткое имя ветки или тега                                         |
| `GITHUB_REF_TYPE`    | Тип ref: `branch` или `tag`                                         |
| `GITHUB_REPOSITORY`  | `owner/repository`                                                  |
| `GITHUB_SHA`         | SHA коммита, запустившего Workflow                                  |
| `GITHUB_WORKFLOW`    | Название Workflow                                                   |
| `GITHUB_RUN_ID`      | Уникальный ID запуска Workflow                                      |
| `GITHUB_RUN_NUMBER`  | Номер запуска Workflow                                              |
| `GITHUB_RUN_ATTEMPT` | Номер попытки запуска при повторном запуске                         |
| `GITHUB_JOB`         | ID текущего Job                                                     |
| `GITHUB_WORKSPACE`   | Рабочая директория репозитория на Runner                            |
| `RUNNER_OS`          | ОС Runner: `Linux`, `Windows` или `macOS`                           |
| `RUNNER_ARCH`        | Архитектура Runner: `X64`, `ARM64` и т.д.                           |
| `RUNNER_ENVIRONMENT` | Тип Runner: `github-hosted` или `self-hosted`                       |