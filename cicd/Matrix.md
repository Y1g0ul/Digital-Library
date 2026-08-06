---
created-dt: 2026-08-06 11:47
tags:
  - review
---
Cпособ запустить один [[Job]] несколько раз
с разными комбинациями заданных значений.
- версиях языка;
- ОС;
- конфигурациях;
- других параметрах.

```yaml
jobs:
  test:
    runs-on: ${{ matrix.os }}

    strategy:
      matrix:
        os:
          - ubuntu-latest
          - windows-latest

        node:
          - 20
          - 22

    steps:
      - name: Test
        run: echo "Testing Node ${{ matrix.node }}"
```

