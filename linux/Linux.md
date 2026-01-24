---
created-dt: "2025-08-17 20:22"
tags:
---

ls -a | grep -v "\.$" выведет все файлы включая скрытые кроме тех что кончаются на точку (\ это экранирование точки)
ls | grep -v "t$" выведет все файлы кроме тех что кончаются на t
ls | grep -v ^output выведет все файлы кроме тех которые начинаются с output

Точка и две точки еди единственные хард линки на каталоги. "." - На себя. ".." - на вышестоящий каталог
Регулярные выражения. 
Регулярное выражение для поиска мак адреса - ip a | grep -P '([0-9a-f]{2}:){5}[0-9a-f]{2}'
[0-9a-f] - цифра от 0 до 9 или буква от a до f.
[0-9a-f]{2} - повторить 2 раза
([0-9a-f]{2}:){5} - конструкцию из 2ух букв/цифр и двоеточия повторить 5 раз
1. `[0-9a-f]` — ищет **один символ** из диапазона: цифры (`0-9`) или буквы `a-f` (это 16-ричная система).
2. `{2}` — ровно **2 таких символа** подряд (например, `00` или `3a`).
3. `:` — после каждой пары символов должна идти **двоеточие**.
4. `(...){5}` — повторяет шаблон в скобках **5 раз** (т.е. `XX:XX:XX:XX:XX:`).
5. Последняя часть `[0-9a-f]{2}` — завершающая пара символов **без двоеточия** (итого: `XX:XX:XX:XX:XX:XX`).
Получается найти мак адрес.
 ip a | grep -P '([0-9a-f]{2}:){5}[0-9a-f]{2}'
link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
link/ether 00:15:5d:0d:ba:36 brd ff:ff:ff:ff:ff:ff

Регулярное выражение для ip адреса. myuser@ruvds-baz2e:~$ ip a | grep -P '((25[0-5]|2[0-4][0-9]|1[0-9]{2}|[0-9]{1,2})\.){3}(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[0-9]{1,2})'
    inet 127.0.0.1/8 scope host lo
    inet 194.87.103.63/23 scope global eth0
Первые 3 числа (с точкой в конце): 
   `(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[0-9]{1,2})\.`  
   - `25[0-5]` → 250–255  
   - `2[0-4][0-9]` → 200–249  
   - `1[0-9]{2}` → 100–199  
   - `[0-9]{1,2}` → 0–99  
   - `\.` → точка после числа.

2. Последнее число (без точки):
   `(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[0-9]{1,2})`  
   Аналогично, но без `\.` в конце.

Apache используется для виртуального хостинга т.к можно вынести файл конфига каждого сайта, дать права для редактирования этого файла и это не повлияет на работу всего окружения.
Nginx cовременный быстрый вею сервер рассчитанный на работу с большим мощным окружением. Может распределять нагрузки.
Обычно делают связку frontend - Nginx, backend - Apache.

myuser@ruvds-baz2e:~$ for i in {2020..2025}; do echo $i; done
2020
2021
2022
2023
2024
2025
for переменная in {начало..конец}; do команда; done


myuser@ruvds-baz2e:~$ for m in {2020..2025}; do echo $m;for y in {1..12}; do echo $y; done; done
2020
1
2
3
4
5
6
7
8
9
10
11
12
2021
1
2
3
4
5
6
7
8
9
10
11
12
2022
1
2
3
4
5
6
7
8
9
10
11
12
2023
1
2
3
4
5
6
7
8
9
10
11
12
2024
1
2
3
4
5
6
7
8
9
10
11
12
2025
1
2
3
4
5
6
7
8
9
10
11
12
Добавили встроенный цикл для вывода месяцев кадого года.
for year in {2015..2020}; do for m in {01..12}; do mkdir -p ./$year/$m; for n in {1..9}; do echo $n.txt > ./$year/$m/$n.txt; done; done; done


for year in {2015..2020}; do for m in {01..12}; do mkdir -p ./$year/$m; for n in {1..9}; do echo $n.txt > ./$year/$m/$n.txt; done; done; done
Создаем каталоги года с 2015 по 2020, в каждом создаем подкаталоги по 12 месяцев, в каждом делаем по 9 txt файлов в нутри которых написано их название

Задача - где-то в каталоге etc есть файлы в которых описан пользователь root. Нужно найти их. 
myuser@ruvds-baz2e:~$  grep -r "^root:x:" /etc/ 2> /dev/null
/etc/group:root:x:0:
/etc/passwd-:root:x:0:0:root:/root:/bin/bash
/etc/passwd:root:x:0:0:root:/root:/bin/bash
/etc/group-:root:x:0:
 Мв поставили ключ -r что означает рекурсивно. Т.к греп может работать с каталогами он прошелся по указанному каталогу "/etc" и рекурсивно просмотрел все файлы.





















dpkg -l список всех установленных программ


apt-cache search server поиск по пакетам по ключевому слову (в нашем случае "server")
netstat -tnlp выводит активные слушающие порты на нашем сервере

root@jsmqwxbulv:~# systemctl status apache2
используется для просмотра состояния службы (сервиса), юнита (unit) или другой сущности, управляемой systemd
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/apache2.service; enabled; preset: >     Active: active (running) since Wed 2025-05-14 12:03:28 UTC; 42min ago
       Docs: https://httpd.apache.org/docs/2.4/
   Main PID: 139568 (apache2)
      Tasks: 55 (limit: 1110)
     Memory: 5.9M (peak: 6.0M)
        CPU: 195ms
     CGroup: /system.slice/apache2.service
             ├─139568 /usr/sbin/apache2 -k start
             ├─139570 /usr/sbin/apache2 -k start
             └─139572 /usr/sbin/apache2 -k start
May 14 12:03:28 jsmqwxbulv systemd[1]: Starting apache2.service - The Apache HT>May 14 12:03:28 jsmqwxbulv systemd[1]: Started apache2.service - The Apache HTT>lines 1-15/15 (END)
systemctl stop apache2
systemctl start apache2


ps -efl список всех работающих процессов на нашем сервере 

systemctl reload apache2 перепрочитает файл конфига
systemctl restart apache2 перезапустит сервис (будет остановка)

grep -i - -i для игнорирования регистра

**`man`** (от *manual*) — это встроенная справочная система Linux/Unix, которая показывает **руководства по командам, функциям и конфигам**.  

apropos - выполнит поиск по ключевым словам в описании утилит
myuser@hcwrjgdfsp:~$ apropos password
chage (1)            - change user password expiry information
chgpasswd (8)        - update group passwords in batch mode
endpwent (3)         - get password file entry
login.defs (5)       - shadow password suite configuration
Обратите внимание на число в скобках после имени команды. Это глава или раздел справочного руководства man, в которых может встречаться описание утилиты. Разделов много, приведем самые полезные:
1 - команды пользователя,
2 - системные вызовы ядра (используется программистами),
© geekbrains.ru
5 - форматы файлов,
8 - команды администрирования.
Эти номера можно использовать в командах man и apropos. Например, passwd - не только команда, но и имя системного файла. Если нам интересен формат этого файла, надо набрать:


### Как работает:  
1. **Вводите команду** (например, `man ls`).  
2. Система ищет **страницу мануала** в стандартных разделах (`/usr/share/man`).  
3. Отображает её в **пейджере** (обычно `less`), где можно:  
   - Листать (**↑/↓**, **PgUp/PgDn**).  
   - Искать (**/** + текст).  
   - Выйти (**q**).  
man grep  # Справка по grep


Оба веб-сервера (Apache и Nginx) используют схожую структуру для управления виртуальными хостами (сайтами), но с небольшими различиями.  

- **Хранит все конфигурации виртуальных хостов** (сайтов), даже те, что не активны.  
Содержит символические ссылки (symlinks) на активные конфиги** из `sites-available`.
`sites-available`** — хранит все возможные конфиги.  
- **`sites-enabled`** — только те, что должны работать прямо сейчас.  

Apache (`/etc/apache2/`)**
### **`sites-available`**  
- **Хранит все конфигурации виртуальных хостов** (сайтов), даже те, что не активны.  
- **Пример файла**:  
  ```bash
  /etc/apache2/sites-available/example.com.conf
  ```
  Содержит настройки для домена `example.com`:
  ```apache
  <VirtualHost *:80>
      ServerName example.com
      DocumentRoot /var/www/example.com
      <Directory /var/www/example.com>
          Options -Indexes +FollowSymLinks
          AllowOverride All
      </Directory>
  </VirtualHost>
  ```

### **`sites-enabled`**  
- **Содержит символические ссылки (symlinks) на активные конфиги** из `sites-available`.  
- **Как включить сайт?**  
  ```bash
  sudo a2ensite example.com.conf  # Создаёт симлинк в sites-enabled
  sudo systemctl reload apache2   # Применяем изменения
  ```
- **Как отключить?**  
  ```bash
  sudo a2dissite example.com.conf  # Удаляет симлинк
  sudo systemctl reload apache2
  ```

### **Почему так сделано?**  
- **`sites-available`** — хранит все возможные конфиги.  
- **`sites-enabled`** — только те, что должны работать прямо сейчас.  
- **`a2ensite`/`a2dissite`** — управляют symlinks без ручного редактирования.  

---

## **2. Nginx (`/etc/nginx/`)**
### **`sites-available`**  
- Аналогично Apache, хранит **все конфиги**, даже неактивные.  
- **Пример файла**:  
  ```bash
  /etc/nginx/sites-available/example.com
  ```
  ```nginx
  server {
      listen 80;
      server_name example.com;
      root /var/www/example.com;
      index index.html;
  }
  ```

### **`sites-enabled`**  
- **Тоже содержит symlinks** на активные конфиги.  
- **Как включить сайт?**  
  ```bash
  sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
  sudo nginx -t  # Проверяем синтаксис
  sudo systemctl reload nginx
  ```
- **Как отключить?**  
  ```bash
  sudo rm /etc/nginx/sites-enabled/example.com
  sudo systemctl reload nginx
  ```

### **Важно!**  
- В Nginx **главный конфиг (`nginx.conf`)** включает `sites-enabled` так:  
  ```nginx
  include /etc/nginx/sites-enabled/*;
  ```
- Если этого нет — сайты из `sites-enabled` **не загрузятся

Настройка Apache и Nginx с проксированием на несколько портов
📁 Структура директорий
В каталоге /var/www/ были созданы 3 подкаталога:
/var/www/8080
/var/www/8081
/var/www/8082
В каждом из них расположен файл index.html, содержащий число, соответствующее номеру порта:

/var/www/8080/index.html → содержит 8080

/var/www/8081/index.html → содержит 8081

/var/www/8082/index.html → содержит 8082

Настройка Apache
Изменение порта прослушивания
В файле /etc/apache2/ports.conf были добавлены строки:
Listen 8080
Listen 8081
Listen 8082
Создание виртуальных хостов
В каталоге /etc/apache2/sites-available/ созданы 3 файла конфигурации:
8080.conf
8081.conf
8082.conf
Пример содержимого файла 8080.conf (для других аналогично с изменённым портом и путем):
<VirtualHost *:8080>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/8080

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
Активация конфигураций
Созданы символьные ссылки в /etc/apache2/sites-enabled/:
8080.conf -> ../sites-available/8080.conf
8081.conf -> ../sites-available/8081.conf
8082.conf -> ../sites-available/8082.conf
Настройка Nginx
Создание upstream-блока
В файле /etc/nginx/sites-available/upstream добавлен следующий upstream:
upstream apache {
    server 127.0.0.1:8080;
    server 127.0.0.1:8081;
    server 127.0.0.1:8082;
}
Настройка проксирования в default-сайте
В файл /etc/nginx/sites-available/default добавлен блок:
location / {
    try_files $uri $uri/ =404;
    proxy_pass http://apache;
}
Активация конфигураций
Созданы символьные ссылки:
default  -> /etc/nginx/sites-available/default
upstream -> /etc/nginx/sites-available/upstream


etc/hosts -файл со всем соответствиями имен и ip адресов 
127.0.0.1       localhost

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost   ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
ff02::3 ip6-allhosts
127.0.1.1       hcwrjgdfsp.local        hcwrjgdfsp

nslookup - команда проверающая есть ли прикрепленный к этому имени ip
nslookup localhost
Server:         198.18.18.18
Address:        198.18.18.18#53

Name:   localhost
Address: 127.0.0.1
Name:   localhost
Address: ::1

etc/resolv.conf - файл в котором прописан dns сервер

Полная виртуализация** → гостевая ОС думает, что работает на реальном железе.  
- **Паравиртуализация** → гостевая ОС знает, что виртуализована, и оптимизирует работу через гипервизор.




boosty - оригинал картинки [https://boosty.to/chilldevops](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbEpOSF8xMV8ycG01RURqYkowb1FxeGZhQllzZ3xBQ3Jtc0tsM0g2UmRQZUlWaUJaU3c3dkRfNXZTZ2JaVnNnSlo0VEc3RldDSW9FYlJRU01OTThGUkJhZElpQkJRWFR4VVFfUUQtekJTdUxTN2dueHJtQ3ZOa19idy1QZ2xWUXVPcnpYR1FFTXRoanRBTHVmTWFiNA&q=https%3A%2F%2Fboosty.to%2Fchilldevops) Obsidian - [https://habr.com/ru/companies/ozonbank/articles/838990/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbFgxX01COHJ4QzM1MHZIMXpjOTRWVXhMU0o0UXxBQ3Jtc0tsZDc0c3BqMHgyajc0VGlpMkpwcDJLZ0xkOXRURi1tc213aDFoOWg3MjgtdVM0Sjh4N3dZQnZLaFpMSXlDbFVKREtrWHF3OFZSZVBWRno3YzlCa0pUeUxKQlJ5M3NyRXFYTjJXLW1xVERzRzhILXBEMA&q=https%3A%2F%2Fhabr.com%2Fru%2Fcompanies%2Fozonbank%2Farticles%2F838990%2F) - [https://habr.com/ru/articles/861230/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqa1dkSVQ4OG1OSkIzMlB1dGxkajBxUjQ3X3RfQXxBQ3Jtc0tsamI2czJZVVFGTlhxalhMNWhfNmJBTlIyQlFTXzVaUml3ci15NUJMU2pIaUNzSlpsU2tSU0xPMHVfOVplNzVfVDFsc2hTeUxLcWZKUmRnVU1uc29pZHNLQlZ5N04zYjJaQ2JCS1dmQV9uU0N5YVRTVQ&q=https%3A%2F%2Fhabr.com%2Fru%2Farticles%2F861230%2F) - [https://habr.com/ru/articles/820669/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqazVrcjdFVDZBWFpjeE4tRkZsVDB2TWpjU202Z3xBQ3Jtc0tuZnRjZ1JiS3Zmd0dFVUZ3Ql9qLVgxcm9FblpZUzZzbzNWd3pER2ZCSkV2YXVYazRlVTl2R3ptd2QzQnVNQi1hdGwxRjExbjZ6NXRuX2Vubm5ZUXpTdGZHT0Y3Tkp5MEFwTVN3SktjSlpPekF6a2p2Yw&q=https%3A%2F%2Fhabr.com%2Fru%2Farticles%2F820669%2F) Linux - Семаев LPIC-1 [https://www.youtube.com/watch?v=rKCu-tfL730&list=PLmxB7JSpraiep6kr802UDqiAIU-76nGfc&ab_channel=KirillSemaev](https://www.youtube.com/watch?v=rKCu-tfL730&list=PLmxB7JSpraiep6kr802UDqiAIU-76nGfc) - Основы GNU/Linux и подготовка к RHCSA [https://www.youtube.com/watch?v=acqAnwP_WZU&list=PLisqB92_b4TlQH3jVGf6lrFMVqalCTjAQ&ab_channel=GNULinuxPro](https://www.youtube.com/watch?v=acqAnwP_WZU&list=PLisqB92_b4TlQH3jVGf6lrFMVqalCTjAQ) - Кетов Как работает Linux [https://www.youtube.com/watch?v=V3gI8-8k8Q4&list=PLHHm04DXWzeKZycf_ZuBgxWdVBnrjE_mj&ab_channel=DmitryKetov](https://www.youtube.com/watch?v=V3gI8-8k8Q4&list=PLHHm04DXWzeKZycf_ZuBgxWdVBnrjE_mj) Git - [atlassian.com/ru/git/tutorials/what-is-git](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbHN5bHFmYXBVOHlmS05xdm9nVUN1Ni1md3dqd3xBQ3Jtc0trdU0yVW1WLVBUd19XUEkycC1OUXRIOXd5NFlXLWtuZk5RMGRzeEpIazdnY1NyVUlhbmhVdFFJZFF2UHBIVlh1VHpzeHFRbUJqZnoxS0dRR0NxR3lRYXZqcjRJZXd5b21JcnVQZ0gyMFgxQWR6MWVUTQ&q=http%3A%2F%2Fatlassian.com%2Fru%2Fgit%2Ftutorials%2Fwhat-is-git) - [https://learngitbranching.js.org/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbEJNcUo4V0dJVGtSWGtUdXZPaUFFOHlOcHhXd3xBQ3Jtc0ttQWFSWERfc3h2Tl9Ndk45b2k4NW5UQm1qanQ0bVdnQkh4Ym9TM1ZBRkJrWG1ubzBSR1EzeHdONjBkbXo0Q2dYWkFETVA1Zm44WVRLQVFuZnM3SnRvNWVNaDNHeGFkcENHRjBtMG1YQUVNbFpja0pETQ&q=https%3A%2F%2Flearngitbranching.js.org%2F) Net/Sec - Сети для самых маленьких [https://linkmeup.gitbook.io/sdsm](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbDBsalFjZ2c2VjFWT21LOWVsc0lYcV9TUWx6d3xBQ3Jtc0tsUkNXUGRtdkhORkNmbnN6MjVfdktXYjlIQjQyVmpzdW1RX2VSRXhMYS1KZWFZdHBKd3BTQ19CRXpmVUh3aVlHS3dhd3ZiVXdSYlo3WEJIS2w1dVBOdjJiVzNtUnpzN2IyRVNrSGkyUHIxZzRmS3Z2SQ&q=https%3A%2F%2Flinkmeup.gitbook.io%2Fsdsm) - tls [https://habr.com/ru/articles/332294/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbENVSV9ZRWVNTWlELU04RXJGWm1CRFM4aVBhd3xBQ3Jtc0tuZXFwSWpaa25JbzBSVE8zTjZHVUpaRVlYMDhIeUhIaGhvRnQyY1pYZG9walRxVkN3S0JPRTcwc2FtOGl2VmMwSHBsWGQzMHFXM3FYeE0zTURqZnNBeWdGRFJuNUlFVmk5a0JTS2s1SEZSR0hHNlFodw&q=https%3A%2F%2Fhabr.com%2Fru%2Farticles%2F332294%2F) - что такое cve [https://habr.com/ru/companies/pvs-studio/articles/678410/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbTFOa29hUDdJYms0R2w3U0c0NGRNTXpMaThoQXxBQ3Jtc0tsbF9YWGpBS2Z5SmtOeTdndC0tcFFnZVJjbW9qUGFHSWVyenIwUmQzS3RTQVZmV0FfUnpXcDVXMWszSk9KaVZMVUVRcklLekJwUDVWa0YyOVRPZXdySk1nUFZWdjcxV3djbkVDSWw4MVVpLUctZlFnTQ&q=https%3A%2F%2Fhabr.com%2Fru%2Fcompanies%2Fpvs-studio%2Farticles%2F678410%2F) Docker - Docker основы [https://habr.com/ru/companies/ruvds/articles/438796/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbXBxaUVkTmZId1pyb3ZJNDd3LU9HbU1HMnZOQXxBQ3Jtc0trZ0UwWW55RldOV3ROQXdCRWk3cmVQaU5POGN1QllqTDAxcFZVMzhwdndFaG83bXVPRFRrVFdOdUVBUUNnT1E4b0xWa25uUDhZS2J2NEV3NTh0S2J6U2I4aXQ3UTNKNkNWMFZmbkwxM2VCZkp1a0FwVQ&q=https%3A%2F%2Fhabr.com%2Fru%2Fcompanies%2Fruvds%2Farticles%2F438796%2F) - Различия между Docker, containerd, CRI-O и runc [https://habr.com/ru/companies/domclick/articles/566224/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqa1ZoY2x3YWlUb3JmOTI5aTB4ZnV6U05FbzNMUXxBQ3Jtc0tubXpEQjVWUzFiVVg0SUROMFR1NXhnYkR5UmdrSDNJQlZBYmRIUXctZWFsREw4SVhuTTVNV3N3REpWb000RlFyQVVxS3NrTEpJOHVGeFJXTklnbFFuVmQ4dF9mR1BGSEtHS0V1SFJuYmVqT0cxQ0lkMA&q=https%3A%2F%2Fhabr.com%2Fru%2Fcompanies%2Fdomclick%2Farticles%2F566224%2F) Monitoring - Годный цикл статей про метрики [https://habr.com/ru/companies/tochka/articles/683608/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbHViYzhWaS1Sb1JMWTdqTmpTdnVjR3NJY0JUZ3xBQ3Jtc0tuY2tmR21zZGxfY2ZCU01vZ2xnMHR4TmpneG10X0k5c1lPTE1RcGxSVXJaMWF0MzNrcXdlMU9qbGhlVmgtUnhNdEhoSnNCRTZWeGdnQTRGcnFzMThLNHZZVXNoRl9Zbk5BNGkxeTFqd0YwS2FmcWlQTQ&q=https%3A%2F%2Fhabr.com%2Fru%2Fcompanies%2Ftochka%2Farticles%2F683608%2F) - Opensearch [https://habr.com/ru/articles/662527/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbjVlbW1nbjEyTUMyU2Q3dUVZSkJZdkVOXzNmQXxBQ3Jtc0ttVTBlQldEelU3dDZDZ3pzcEZkM1FLaWlaSVpBeGRJVmd5SkNrc0Jhb011VG5QRlRscVlscVJIWnpqUF9DdzBSWTRlOUd6a2lDYm1TYWVuWExZVjFFQjlmb1dKTjNJX1pGSV9oZzNYNHhkcy1JR1hmTQ&q=https%3A%2F%2Fhabr.com%2Fru%2Farticles%2F662527%2F) - Prometheus и Grafana [https://habr.com/ru/articles/709204/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbmlLUVFuenNSTDVOcmpYNDdJQTFmaGdyRmRZQXxBQ3Jtc0tsTlZZblRad2dNd0FpR3RYZ3BndS1Icm9SSWlPaFVTNFRrRU45c29TS2k3U3pRd1FpaGlKNE00RzR0OTBSQzFEd2I4MEd2T2h3dTNCUklDeTBYMUg3cW5hd3NudF9PZ3A4azU2d0RsMlRpWkZsOXotbw&q=https%3A%2F%2Fhabr.com%2Fru%2Farticles%2F709204%2F) CI/CD - Gitlab CI [https://habr.com/ru/articles/498436/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbTVDa3lhdUlDT0ZzSnZ2OTRxWjFrOURnejY2UXxBQ3Jtc0tsTHRlQ0tUTWhZTnQ4dkpySzg3OG5uak1VTklub0xtWDRLdXdRaHd6SXctbnJ2Vk10RmtjeF9PYmlPVFlTV0VOVUUtQ2lfeEo5TWlZQWE0T2RGTWVTcTYydVRkU0pHZXh0dWZnaDhfSkdLblJsNG12bw&q=https%3A%2F%2Fhabr.com%2Fru%2Farticles%2F498436%2F) IaC - ADV-IT Ansible [https://www.youtube.com/watch?v=Ck1SGolr6GI&list=PLg5SS_4L6LYufspdPupdynbMQTBnZd31N&ab_channel=ADV-IT](https://www.youtube.com/watch?v=Ck1SGolr6GI&list=PLg5SS_4L6LYufspdPupdynbMQTBnZd31N&pp=0gcJCTAAlc8ueATH) k8s - Slurm Kubernetes для разработчиков [https://www.youtube.com/watch?v=Mw_rEH2pElw&list=PL8D2P0ruohOBSA_CDqJLflJ8FLJNe26K-&ab_channel=%D0%A1%D0%BB%D1%91%D1%80%D0%BC](https://www.youtube.com/watch?v=Mw_rEH2pElw&list=PL8D2P0ruohOBSA_CDqJLflJ8FLJNe26K-&pp=0gcJCTAAlc8ueATH) - Артур Крюков [https://www.youtube.com/@OldPythonKAA/playlists](https://www.youtube.com/@OldPythonKAA/playlists) - helm [https://habr.com/ru/articles/769046/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbE5kb0g2cWlNMFNRbjdvTFFJWVNZbmo2UEtad3xBQ3Jtc0tuMjliM1JYVWtQYzJLbVgyX0tkNmwzR0MtaHVvbzg5MWFncGZEVTNIeWNScTBYa0hsR3YxU1lfb0ZzMUVjS0R6MFMyNzVpSzloZzRaN1FqdzR3bUUxcy03MElRZHdNUmpCTDBlaTJERE90UzZ3cnJvTQ&q=https%3A%2F%2Fhabr.com%2Fru%2Farticles%2F769046%2F) - vault база [https://habr.com/ru/companies/jetinfosystems/articles/762194/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbXlzb29kSW5hVzh3OVRwWUJmMDB0N2V0ZkdoZ3xBQ3Jtc0ttdUVVOUxPZGlZYTd5MGRZXzdKbGdDcy1qVHhodEtENXIzOGFybVduVzF0ZGN0Mng2NGMybTJCUk5jb0V0Q0xzd2w4dlRWdm53SDdWQTB4UnROY2d6WDV4T0xnVDhnQjBiQnFXdHoxWUN3VERQLW13bw&q=https%3A%2F%2Fhabr.com%2Fru%2Fcompanies%2Fjetinfosystems%2Farticles%2F762194%2F) - vault с k8s [https://habr.com/ru/companies/ru_mts/articles/880594/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqa0JYQnItdUp1eTBMZXNLTEwyamJ6MENsR0ZWUXxBQ3Jtc0trQnphMmFEcHRhR1JjTzhwV1Q3clZqaE1MTktTZnY5YzYwU01TRF9VMWh1Zm1xVV9zbHN2RGQxUFQ2cHdtcVpETWdFRmFOeHNyTHdyUFluamJHS1ZyZzdfWEM1LXR4eTRqUDhJSmtveTJmVUtIR3pGQQ&q=https%3A%2F%2Fhabr.com%2Fru%2Fcompanies%2Fru_mts%2Farticles%2F880594%2F) - argocd [https://habr.com/ru/companies/kts/articles/723760/](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqbXhEYU1IMkdGSUhIc3hacXJleU1LT21ubk4wd3xBQ3Jtc0ttU3ppNWE2VkNQUzFya2RDUEZxQXFLa1VuNlR2b0g1TWJ3VGVyTEtKdG9aSEhCbm44ZVdya1BkNlZLSG5hcm9jQ0E5dFRPNHJTY05XMmVaeFVralotbnE5Ym1Yd0NpcGtVZFNGNjVEOGFyVmhrZlBCcw&q=https%3A%2F%2Fhabr.com%2Fru%2Fcompanies%2Fkts%2Farticles%2F723760%2F) Clouds - ADV-IT Terraform [https://www.youtube.com/watch?v=R0CaxXhrfFE&list=PLg5SS_4L6LYujWDTYb-Zbofdl44Jxb2l8&ab_channel=ADV-IT)](https://www.youtube.com/watch?v=R0CaxXhrfFE&list=PLg5SS_4L6LYujWDTYb-Zbofdl44Jxb2l8) - Terraform YaCloud [https://www.youtube.com/watch?v=y1eqR0xL1ZI&ab_channel=loftblog](https://www.youtube.com/watch?v=y1eqR0xL1ZI)