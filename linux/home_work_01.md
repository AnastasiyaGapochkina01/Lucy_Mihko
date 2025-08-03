# Теория
1) Про архитектуру чуть подробнее: https://youtu.be/IyDs9ZgWcTo
2) Директории: https://youtu.be/IPSfoerrNig
3) Файлы: https://youtu.be/fqWjveB1Oyo
4) Работа с текстом: https://youtu.be/MN1vu7haaO8
5) CLI: https://youtu.be/PnvG2DjFCRI
# Практика
1) В домашней директории создать директорию `linux-homeworks`
2) В `linux-homeworks` создать директорию `text-processing` и перейти в нее
2) Создать файл `info.csv` с таким содержимым
```csv
Name:Company:Price:Ganre:Multiplayer
Item1:Company1:60000$:Ganre1:Yes
Item2:Company2:70000$:Ganre2:Yes
Item3:Company3:90000$:Ganre1:Yes
Item4:Company4:90000$:Ganre1:No
Item5:Company5:110000$:Ganre1:Yes
Item6:Company6:410000$:Ganre2:Yes
Item7:Company1:60000$:Ganre1:Yes
Item8:Company4:40000$:Ganre2:No
Item9:Company3:90000$:Ganre1:Yes
```
3) В файле `info.csv` с помощью найти строки, которые содержат слово `No`
4) В файле `info.csv` найти все строки, в которых встречается слово `Ganre2`
5) В файле `info.csv` найти все строки, в которых встречается слово `Ganre2` и вывести для них значение столбца `Price`
6) Создать файл `metrics` с содержимым
```csv
metric_name:metric_type:interval
cache_disk_use:gauge:1
cache_use:gauge:0
key_buffer_bytes_used:byte:3
table.rows.read:count:10
disk_use:gauge:10
data_size:gauge:1
rows_deleted:count:1
bytes_received:byte:1
```
7) В файле `metrics` найти все строки, в которых встречается слово `gauge` и вывести для них значение столбца `metric_name`
8) В файле `metrics` найти все строки, в которых встречается слово `byte` и вывести для них значение столбца `interval`
9) В файле `metrics` найти все строки, в которых встречается слово `disk` и вывести для них значение столбца `metric_type`
10) В файле `metrics` найти все строки, которые *начинаются* с `cache`
11) В файле `info.csv` найти все строки, которые содержат слово `Company4`
12) Отфильтровать вывод команды `free -h` так, чтобы получить размер оперативной памяти
13) Из вывода команды `df -Th` получить сколько процентов места используется на диске/разделе под `/`
14) Получить из вывода команды `uptime` _значения_ load-average
15) Получить из вывода команды `lscpu` количество ядер
16) Из вывода команды `env` получить значения SHELL, PWD и USER
17) Выполнить `sudo netstat -tulpn` и отфильтровать вывод по ssh (если netstat не сработал, то использовать `sudo ss -tulpn`)
18) В директории `text_processing` создать файл `hosts` с содержимым
```csv
hostname,interval,timestamp,%user,%system,%memory
sel-srv1,600,2023-10-08 00:00:01 UTC,30.01,20.77,96.21
sel-srv1,600,2023-10-08 00:05:01 UTC,40.07,13.00,84.55
sel-srv1,600,2023-10-08 00:10:01 UTC,5.00,90.55,89.23
sel-srv2,600,2023-10-09 00:15:01 UTC,25.01,15.77,92.21
sel-srv3,600,2023-10-09 00:20:01 UTC,35.07,10.00,80.55
sel-srv3,600,2023-10-09 00:25:01 UTC,10.00,85.55,88.23
sel-srv2,600,2023-10-09 00:30:01 UTC,20.01,25.77,95.21
sel-srv1,600,2023-10-09 00:40:01 UTC,15.00,10.00,87.23
sel-srv2,600,2023-10-09 00:35:01 UTC,45.07,12.00,82.55
sel-srv3,600,2023-10-09 00:40:01 UTC,15.00,80.55,87.23
```
19) Найти в файле `hosts`
- строки, содержащие `sel-srv2` и вывести для них значение столбца `%memory`
- строки, содержащие конкретную дату `2023-10-08`
- строки, у которых значение `%system` равно `10.00` и вывести названия серверов
20) Удалить все комментарии (строки, начинающиеся с #) из файла /etc/ssh/sshd_config
21) Извлечь содержимое тегов `<title>` из файла `data.xml`
```xml
<book>
  <title>Linux Basics</title>
  <title>Advanced Regex</title>
</book>
```
22) В файле `/etc/passwd` найти пользователей с UID меньше 1000, но исключить системных пользователей (root, daemon, bin).
23) Из лога Apache извлечь все запросы `POST` к `/api/` с кодами ответа `5xx`; пример файла
```
192.168.1.5 - - [10/Jan/2023:12:00:01] "POST /api/v1/login HTTP/1.1" 503 1024
10.0.0.23 - - [10/Jan/2023:12:00:02] "GET /home HTTP/1.1" 200 378
172.16.0.44 - - [10/Jan/2023:12:00:03] "PUT /api/v2/users HTTP/1.1" 201 2150
192.168.1.5 - - [10/Jan/2023:12:00:04] "DELETE /resource/9 HTTP/1.1" 403 512
10.0.0.17 - - [10/Jan/2023:12:00:05] "PATCH /settings/profile HTTP/1.1" 401 1200
172.16.0.44 - - [10/Jan/2023:12:00:06] "POST /api/v1/login HTTP/1.1" 503 1024
192.168.1.20 - - [10/Jan/2023:12:00:07] "GET /products HTTP/1.1" 404 87
10.0.0.23 - - [10/Jan/2023:12:00:08] "PUT /api/v2/items HTTP/1.1" 500 3120
172.16.0.10 - - [10/Jan/2023:12:00:09] "DELETE /resource/5 HTTP/1.1" 200 768
192.168.1.5 - - [10/Jan/2023:12:00:10] "GET /about HTTP/1.1" 200 1450
10.0.0.17 - - [10/Jan/2023:12:00:11] "POST /api/v1/register HTTP/1.1" 201 890
172.16.0.44 - - [10/Jan/2023:12:00:12] "PUT /api/v2/config HTTP/1.1" 400 560
192.168.1.20 - - [10/Jan/2023:12:00:13] "PATCH /user/permissions HTTP/1.1" 403 1250
10.0.0.23 - - [10/Jan/2023:12:00:14] "GET /contact HTTP/1.1" 200 432
172.16.0.10 - - [10/Jan/2023:12:00:15] "DELETE /resource/7 HTTP/1.1" 404 210
192.168.1.5 - - [10/Jan/2023:12:00:16] "POST /api/v1/logout HTTP/1.1" 200 780
10.0.0.17 - - [10/Jan/2023:12:00:17] "GET /dashboard HTTP/1.1" 503 3400
172.16.0.44 - - [10/Jan/2023:12:00:18] "PUT /api/v2/roles HTTP/1.1" 201 1560
192.168.1.20 - - [10/Jan/2023:12:00:19] "PATCH /system/settings HTTP/1.1" 401 920
10.0.0.23 - - [10/Jan/2023:12:00:20] "DELETE /resource/12 HTTP/1.1" 200 640
172.16.0.10 - - [10/Jan/2023:12:00:21] "GET /api/v1/status HTTP/1.1" 500 2100
192.168.1.5 - - [10/Jan/2023:12:00:22] "POST /api/v1/token HTTP/1.1" 400 480
10.0.0.17 - - [10/Jan/2023:12:00:23] "PUT /user/profile HTTP/1.1" 403 1120
172.16.0.44 - - [10/Jan/2023:12:00:24] "GET /help HTTP/1.1" 200 670
192.168.1.20 - - [10/Jan/2023:12:00:25] "PATCH /app/config HTTP/1.1" 404 320
```
24) Из того же файла лога Apache что и в задании 23 вывести количество запросов с каждого ip-адреса
