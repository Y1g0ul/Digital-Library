---
created-dt: 2026-08-03 11:11
tags:
  - review
---
Команда в [[Linux]] для управления современным межсетевым экраном (Firewall) ядра Linux. Является преемником [[iptables]] и использует единый синтаксис для IPv4 и IPv6.

``` bash
nft <команда> <объект> [условия] [действие]
```

| Команда   | Что делает                  |
| --------- | --------------------------- |
| `list`    | показать правила и объекты  |
| `add`     | добавить объект или правило |
| `delete`  | удалить объект или правило  |
| `flush`   | очистить правила            |
| `insert`  | вставить правило            |
| `replace` | заменить правило            |

|Объект|Что делает|
|---|---|
|`table`|контейнер для цепочек и правил|
|`chain`|цепочка правил|
|`rule`|отдельное правило|
|`set`|набор адресов, портов и других значений|

|Действие|Что делает|
|---|---|
|`accept`|разрешить пакет|
|`drop`|молча отбросить пакет|
|`reject`|отбросить пакет и отправить ответ|
|`dnat`|изменить адрес назначения|
|`snat`|изменить адрес источника|
|`masquerade`|SNAT с динамическим IP|
``` bash
nft list ruleset
# показать все правила

nft list tables
# показать таблицы

nft list table inet filter
# показать конкретную таблицу

nft add table inet filter
# создать таблицу

nft add chain inet filter input '{ type filter hook input priority 0; policy drop; }'
# создать цепочку INPUT с политикой DROP

nft add rule inet filter input tcp dport 22 accept
# разрешить SSH

nft add rule inet filter input ip saddr 192.168.1.10 drop
# заблокировать IP

nft flush ruleset
# удалить все правила
```

