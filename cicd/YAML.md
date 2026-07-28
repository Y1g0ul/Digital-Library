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


``` yaml
# Комментарий

# Ключ: значение
name: My Project
version: 1
enabled: true
price: 15.5

# null
description: null

# Строки
short: Hello
quoted: "Hello World"
multiline: |
  Первая строка
  Вторая строка
  Третья строка

folded: >
  Эта строка
  будет объединена
  в одну.

# Список
ports:
  - 80
  - 443
  - 8080

# Список объектов
users:
  - name: Nikita
    role: admin

  - name: Alex
    role: developer

# Словарь (Map)
database:
  host: localhost
  port: 5432
  user: postgres
  password: secret

# Вложенные структуры
app:
  name: API
  replicas: 3

  resources:
    cpu: "500m"
    memory: "512Mi"

# Пустой список
volumes: []

# Пустой объект
labels: {}

# Массив внутри объекта
service:
  ports:
    - 80
    - 443

# Объект внутри массива
containers:
  - name: nginx
    image: nginx:latest

    env:
      - name: ENV
        value: production

      - name: DEBUG
        value: "false"
```

