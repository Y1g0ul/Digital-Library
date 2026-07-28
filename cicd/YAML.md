YAML Ain't Markup Language - язык сериализации данных, используемый для хранения конфигурации в удобном для чтения виде.

В DevOps YAML применяется для описания инфраструктуры, [[Pipeline]] и конфигурационных файлов.

Основные правила

**`Отступы`**
YAML использует пробелы, а не фигурные скобки или табуляцию.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```
> Используйте только пробелы. Табуляция (`Tab`) недопустима.

**`Ключ: значение`**
```yaml
name: Build
```

**`Списки`**
```yaml
steps:
  - name: Checkout
  - name: Build
  - name: Test
```

**`Вложенность`**
```yaml
job:
  steps:
    - name: Build
```
Вложенность определяется количеством пробелов.

**`Типы данных`**

Строка
```yaml
name: Build
```

Число
```yaml
version: 3
```

Логическое значение
```yaml
enabled: true
```

Список
```yaml
ports:
  - 80
  - 443
```

Словарь
```yaml
database:
  host: localhost
  port: 5432
```
