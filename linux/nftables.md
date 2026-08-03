---
created-dt: 2026-08-03 11:11
tags:
  - review
---
Современная система [[Linux]] для управления межсетевым экраном (Firewall) ядра Linux. Является преемником `iptables` и используется для фильтрации, изменения и перенаправления сетевых пакетов.

Основная структура:

```text
nftables
   │
   └── Table
        │
        ├── Chain
        │    ├── Rule
        │    ├── Rule
        │    └── Rule
        │
        └── Chain
             └── Rule
```

### Table

**Table (таблица)** — контейнер для правил Firewall. Таблица принадлежит определённому семейству протоколов (`ip`, `ip6`, `inet` и т.д.).

```bash
nft add table inet filter
```

| Семейство | Назначение |
|---|---|
| `ip` | IPv4 |
| `ip6` | IPv6 |
| `inet` | IPv4 + IPv6 |
| `arp` | ARP |
| `bridge` | трафик bridge |

Посмотреть таблицы:

```bash
nft list tables
```

Удалить таблицу:

```bash
nft delete table inet filter
```

---

### Chain

**Chain (цепочка)** — набор правил, которые проверяются последовательно.

Основные цепочки:

| Chain | Назначение |
|---|---|
| `input` | входящий трафик к компьютеру |
| `output` | исходящий трафик от компьютера |
| `forward` | трафик, проходящий через компьютер |
| `prerouting` | обработка до маршрутизации |
| `postrouting` | обработка после маршрутизации |

Создание цепочки:

```bash
nft add chain inet filter input
```

Для обычной фильтрации входящего трафика цепочка должна иметь тип, hook и приоритет:

```bash
nft 'add chain inet filter input { type filter hook input priority 0; policy drop; }'
```

Здесь:

```text
type filter
→ цепочка используется для фильтрации

hook input
→ обрабатывает входящие пакеты

priority 0
→ порядок обработки цепочки

policy drop
→ по умолчанию запрещать пакеты
```

---

### Rule

**Rule (правило)** — конкретное условие и действие над пакетом.

Например:

```bash
nft add rule inet filter input tcp dport 22 accept
```

Означает:

```text
входящий пакет
      ↓
TCP?
      ↓
порт 22?
      ↓
ACCEPT
```

Разрешить HTTP и HTTPS:

```bash
nft add rule inet filter input tcp dport 80 accept
nft add rule inet filter input tcp dport 443 accept
```

Заблокировать IP:

```bash
nft add rule inet filter input ip saddr 192.168.1.10 drop
```

---

### Основные условия

| Условие | Что проверяет |
|---|---|
| `ip saddr` | IPv4-адрес источника |
| `ip daddr` | IPv4-адрес назначения |
| `ip6 saddr` | IPv6-адрес источника |
| `tcp dport` | TCP-порт назначения |
| `tcp sport` | TCP-порт источника |
| `udp dport` | UDP-порт назначения |
| `iifname` | входящий интерфейс |
| `oifname` | исходящий интерфейс |

Примеры:

```bash
nft add rule inet filter input tcp dport 22 accept
# разрешить TCP-порт 22

nft add rule inet filter input ip saddr 192.168.1.10 drop
# заблокировать IP

nft add rule inet filter input iifname "eth0" accept
# разрешить трафик с интерфейса eth0
```

---

### Основные действия

| Действие | Что делает |
|---|---|
| `accept` | разрешить пакет |
| `drop` | молча отбросить пакет |
| `reject` | отбросить пакет и отправить ответ |
| `dnat` | изменить адрес назначения |
| `snat` | изменить адрес источника |
| `masquerade` | SNAT с динамическим IP |

---

### Просмотр правил

```bash
nft list ruleset
# показать всю конфигурацию nftables

nft list tables
# показать таблицы

nft list table inet filter
# показать конкретную таблицу

nft list chain inet filter input
# показать конкретную цепочку
```

---

### Удаление правил

Посмотреть правила с handle:

```bash
nft -a list chain inet filter input
```

Пример:

```text
tcp dport 22 accept comment "SSH" # handle 5
```

Удалить правило:

```bash
nft delete rule inet filter input handle 5
```

Очистить цепочку:

```bash
nft flush chain inet filter input
```

Очистить всю таблицу:

```bash
nft flush table inet filter
```

---

## NAT

NAT используется для изменения адресов и портов пакетов.

### DNAT — проброс порта

Например:

```text
SERVER:2222
     ↓
192.168.1.10:22
```

Пример:

```bash
nft add table ip nat

nft 'add chain ip nat prerouting { type nat hook prerouting priority -100; }'

nft add rule ip nat prerouting tcp dport 2222 dnat to 192.168.1.10:22
```

Теперь подключение к порту `2222` перенаправляется на `192.168.1.10:22`.

### REDIRECT

Перенаправляет трафик на другой порт **этого же компьютера**:

```text
80
 ↓
8080
```

```bash
nft add rule ip nat prerouting tcp dport 80 redirect to :8080
```

### MASQUERADE

Используется, когда внутренние устройства выходят в интернет через Linux-сервер:

```text
192.168.1.10
      ↓
Linux
      ↓
Internet
```

```bash
nft add rule ip nat postrouting oifname "eth0" masquerade
```

---

## nftables и iptables

`nftables` — современная система Firewall.

`iptables` — старый интерфейс, который во многих современных Linux работает через совместимый слой `iptables-nft`.

Проверить:

```bash
iptables -V
```

Если вывод содержит:

```text
iptables v1.8.x (nf_tables)
```

значит `iptables` использует backend `nftables`.

Реальные правила можно посмотреть:

```bash
nft list ruleset
```

> **iptables** — старый синтаксис управления Firewall.  
> **nftables** — современная система, с которой стоит работать при создании новых правил.