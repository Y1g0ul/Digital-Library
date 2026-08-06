[[Workflow]], который можно вызывать из другого Workflow
и использовать повторно. Позволяет вынести общую CI/CD-логику в отдельный Workflow.

Cделать Workflow переиспользуемым
```yaml
on:
  workflow_call:
```

В вызывающем Workflow:
``` bash
jobs:
  call:
    uses: ./.github/workflows/reusable.yml
```
Для Workflow из другого репозитория:
``` yml
jobs:
  call:
    uses: owner/repository/.github/workflows/reusable.yml@main
```

**`Передача Inputs`**
В reusable Workflow:
``` yml
on:
  workflow_call:
    inputs:
      title:
        type: string
        required: false
        default: "Default title"
```
При вызове:
``` yml
jobs:
  call:
    uses: ./.github/workflows/reusable.yml
    with:
      title: ${{ inputs.title }}
```
Внутри reusable Workflow:
``` yml
${{ inputs.title }}
```

Передача Secrets
В reusable Workflow:
``` yml
on:
  workflow_call:
    secrets:
      PASSWORD:
        required: true
```
При вызове:
``` yml
jobs:
  call:
    uses: owner/repository/.github/workflows/reusable.yml@main
    secrets:
      PASSWORD: ${{ secrets.PASSWORD }}
```
Внутри:
``` yml
${{ secrets.PASSWORD }}
```