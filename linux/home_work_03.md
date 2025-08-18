1) Создать файлы
- `shop-access.log`
```
192.168.1.10 - alice [18/Aug/2023:14:30:01 +0000] "GET /home HTTP/1.1" 200 1234
10.0.0.5 - bob [18/Aug/2023:14:30:02 +0000] "POST /login HTTP/1.1" 401 567
172.16.0.12 - charlie [18/Aug/2023:14:30:03 +0000] "GET /dashboard HTTP/1.1" 200 2048
192.168.1.15 - diana [18/Aug/2023:14:30:04 +0000] "GET /profile HTTP/1.1" 404 321
10.0.0.8 - eric [18/Aug/2023:14:30:05 +0000] "POST /upload HTTP/1.1" 500 789
192.168.1.20 - frank [18/Aug/2023:14:30:06 +0000] "GET /settings HTTP/1.1" 200 654
172.16.0.14 - grace [18/Aug/2023:14:30:07 +0000] "DELETE /account HTTP/1.1" 403 210
192.168.1.25 - helen [18/Aug/2023:14:30:08 +0000] "PUT /update HTTP/1.1" 201 432
10.0.0.12 - ivan [18/Aug/2023:14:30:09 +0000] "GET /home HTTP/1.1" 200 1122
172.16.0.18 - john [18/Aug/2023:14:30:10 +0000] "POST /comments HTTP/1.1" 200 777
192.168.1.30 - kate [18/Aug/2023:14:30:11 +0000] "GET /images/logo.png HTTP/1.1" 200 5231
10.0.0.15 - leo [18/Aug/2023:14:30:12 +0000] "GET /docs/api.pdf HTTP/1.1" 404 98
172.16.0.20 - mona [18/Aug/2023:14:30:13 +0000] "PATCH /password HTTP/1.1" 200 344
192.168.1.35 - nick [18/Aug/2023:14:30:14 +0000] "GET /about HTTP/1.1" 200 876
10.0.0.18 - olga [18/Aug/2023:14:30:15 +0000] "POST /checkout HTTP/1.1" 302 221
172.16.0.22 - paul [18/Aug/2023:14:30:16 +0000] "GET /cart HTTP/1.1" 200 954
192.168.1.40 - quinn [18/Aug/2023:14:30:17 +0000] "GET /faq HTTP/1.1" 200 411
10.0.0.20 - rita [18/Aug/2023:14:30:18 +0000] "POST /register HTTP/1.1" 409 623
172.16.0.25 - sam [18/Aug/2023:14:30:19 +0000] "GET /download/setup.exe HTTP/1.1" 200 8123
192.168.1.45 - tina [18/Aug/2023:14:30:20 +0000] "HEAD /ping HTTP/1.1" 200 0
```
- `auth.log`
```
Aug 18 14:35:01 server sshd[12345]: Failed password for root from 10.0.0.1 port 22
Aug 18 14:35:05 server sshd: Accepted password for alice from 192.168.1.10 port 55
Aug 18 14:35:10 server sshd: Failed password for bob from 172.16.0.5 port 2222
Aug 18 14:35:15 server sshd: Failed password for invalid user test from 203.0.113.12 port 445
Aug 18 14:35:20 server sshd: Accepted publickey for diana from 192.168.1.20 port 5032
Aug 18 14:35:25 server sshd: Failed password for eric from 10.0.0.8 port 6001
Aug 18 14:35:30 server sshd: Accepted password for frank from 172.16.0.7 port 2200
Aug 18 14:35:35 server sshd: Failed password for root from 10.0.0.9 port 2222
Aug 18 14:35:40 server sshd: Accepted password for grace from 192.168.1.25 port 715
Aug 18 14:35:45 server sshd: Failed password for helen from 203.0.113.55 port 443
Aug 18 14:35:50 server sshd: Accepted password for ivan from 172.16.0.10 port 1234
Aug 18 14:35:55 server sshd: Failed password for john from 10.0.0.15 port 8022
Aug 18 14:36:00 server sshd: Failed password for invalid user guest from 192.0.2.100 port 1999
Aug 18 14:36:05 server sshd: Accepted publickey for kate from 192.168.1.30 port 2223
Aug 18 14:36:10 server sshd: Accepted password for leo from 10.0.0.22 port 3333
Aug 18 14:36:15 server sshd: Failed password for mona from 172.16.0.12 port 8090
Aug 18 14:36:20 server sshd: Failed password for nick from 203.0.113.77 port 2345
Aug 18 14:36:25 server sshd: Accepted password for olga from 10.0.0.25 port 9123
Aug 18 14:36:30 server sshd: Accepted publickey for paul from 192.168.1.40 port 10000
Aug 18 14:36:35 server sshd: Failed password for tina from 172.16.0.14 port 8081
```
- `system_errors.log`
```
2023-08-18 14:40:01 [ERROR] Service "nginx" failed to start (code=1)
2023-08-18 14:42:05 [WARNING] Disk /dev/sda1 at 85% capacity
2023-08-18 14:42:10 [INFO] Service "ssh" started successfully (pid=2345)
2023-08-18 14:42:15 [ERROR] Service "postgres" crashed with signal 9
2023-08-18 14:42:20 [WARNING] High memory usage detected (92%)
2023-08-18 14:42:25 [INFO] Backup job "daily_backup" completed in 132s
2023-08-18 14:42:30 [CRITICAL] Kernel panic detected, system reboot required
2023-08-18 14:42:35 [INFO] User "alice" logged in from 192.168.1.10
2023-08-18 14:42:40 [ERROR] docker container "web_app" exited unexpectedly (code=137)
2023-08-18 14:42:45 [WARNING] Disk /dev/sdb1 SMART status: WARNING
2023-08-18 14:42:50 [INFO] Scheduled task "cleanup" started
2023-08-18 14:42:55 [INFO] Scheduled task "cleanup" finished successfully
2023-08-18 14:43:00 [ERROR] Authentication failure for user "root" via ssh
2023-08-18 14:43:05 [INFO] Service "redis" started successfully (pid=4533)
2023-08-18 14:43:10 [CRITICAL] RAID array degraded: /dev/md0
2023-08-18 14:43:15 [WARNING] Network latency towards 8.8.8.8 above threshold (250ms)
2023-08-18 14:43:20 [INFO] Package "openssl" updated successfully
2023-08-18 14:43:25 [ERROR] Service "mysql" failed to bind to port 3306 (address in use)
2023-08-18 14:43:30 [WARNING] CPU temperature high: 88°C
2023-08-18 14:43:35 [INFO] System shutdown initiated by user "admin"
```
2) Извлечь все ip-адреса и имена пользователей из файла `shop-access.log`
3) Найти 5 самых частых кодов ответа HTTP в `shop-access.log`
4) Получить список пользователей с неудачными попытками входа из `auth.log`
5) Подсчитать количество ERROR и WARNING в `system_errors.log`
6) Извлечь домены из URL в `shop-access.log`
7) Найти все записи в `auth.log` с IP 10.0.0.1 за последние 24 часа (`find /var/log -name "auth.log*" -mtime -1`)
8) Вывести список сервисов с ошибками из `system_errors.log`
9) Получить топ-3 самых активных IP по количеству запросов в `shop-access.log`
10) Извлечь все MAC-адресы из вывода `arp -a`
11) Найти файлы в `/var/log`, содержащие слово OOM (Out of Memory)
12) Извлечь версии ОС из вывода `cat /etc/os-release`
13) Из вывода ss -tuln извлечь все порты TCP в состоянии LISTEN
