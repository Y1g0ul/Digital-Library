---
created-dt: 2026-08-05 11:51
tags:
  - review
---
Это сохранение файлов между запусками [[Workflow]], чтобы не скачивать/создавать их заново.

Например, вместо того чтобы каждый раз скачивать npm-зависимости:

```text
Runner → Internet → npm packages
```

можно сохранить npm cache:

```text
Первый запуск:
Runner → Internet → ~/.npm → GitHub Cache

Следующий запуск:
GitHub Cache → Runner → npm install
```

---

## `actions/cache`

```yaml
- uses: actions/cache@v6
  with:
    path: ~/.npm
    key: ${{ runner.os }}-npm-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-npm-
```

### Главное запомнить

```text
path
↓
ЧТО сохраняем?

key
↓
КАКОЙ это кэш?

restore-keys
↓
Какой похожий кэш попробовать найти,
если точного нет?
```

### `key`

Обычно ключ зависит от файлов, которые определяют содержимое кэша:

```yaml
key: ${{ runner.os }}-npm-${{ hashFiles('**/package-lock.json') }}
```

Если `package-lock.json` изменился → изменится hash → изменится `key` → будет создан новый кэш.

### Проверка попадания в кэш

Если у step есть:

```yaml
id: cache
```

можно проверить:

```yaml
run: echo "Cache hit: ${{ steps.cache.outputs.cache-hit }}"
```

`true` → точный кэш найден.

---

## Простая схема

```text
             GitHub Cache
                  │
            restore / save
                  │
                  ▼
            Ubuntu Runner
                  │
                  ▼
             npm install
```

**Запомни:** Cache нужен, чтобы **ускорить повторные запуски workflow**, сохраняя то, что не нужно каждый раз скачивать или создавать заново.