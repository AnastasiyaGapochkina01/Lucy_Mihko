1) Создать файлы
- fruits_and_vegatables.csv
```csv
title:count:price
apple:5:2$
banana:3:5$
carrot:2:1$
potato:4:0.5$
tomato:7:1$
orange:6:3$
lemon:8:4$
cucumber:5:1$
onion:3:0.3$
garlic:10:5$
lemon2:9:4$
```
- users.txt
```txt
John Doe: john@example.com (2023-01-15)
Jane Smith: jane@test.com (2023-02-20)
Alice Johnson: alice@example.org (2023-03-25)
Bob Brown: bob@brown.com (2023-04-10)
Carol White: carol@white.org (2023-05-15)
Dave Black: dave@black.net (2023-06-20)
Eve Green: eve@green.io (2023-07-25)
Frank Blue: frank@blue.co (2023-08-30)
Grace Gray: grace@gray.edu (2023-09-05)
Helen Red: helen@red.tv (2023-10-12)
```
- logs.txt
```txt
[2023-01-01 10:00:00] INFO: User 'admin' logged in from 192.168.1.1
[2023-01-01 10:15:23] ERROR: Connection failed from 10.0.0.2
[2023-01-01 11:30:45] INFO: Data saved by 'user_john' from 192.168.1.3
[2023-01-02 09:12:34] WARNING: Disk space low on server 192.168.1.100
[2023-01-02 10:11:12] INFO: Backup started from 10.0.0.5
[2023-01-03 14:15:16] ERROR: Invalid input from 'test_user' from 192.168.1.15
[2023-01-03 15:20:30] INFO: User 'admin' logged out from 192.168.1.1
[2023-01-04 08:45:00] INFO: System reboot by 192.168.1.50
[2023-01-04 12:30:15] WARNING: High memory usage on 192.168.1.100
[2023-01-05 17:45:00] INFO: Update applied from 10.0.0.7
```
2) Подсчитать общее количество строк во всех файлах
3) Найти строки с ошибками (ERROR) в `logs.txt`
4) Извлечь все IP-адреса из `logs.txt`
5) Найти все email-адреса в `users.txt`
6) Вывести из `logs.txt` все строки с "INFO" и заменить "INFO" на "INFORMATION"
7) Вывести строки из `fruits_and_vegatables.csv`, где значение столбца count > 5
8) Подсчитать количество уникальных пользователей в `logs.txt`
9) Найти строки в `fruits_and_vegatables.csv`, где название начинается с гласной (a, e, i, o, u)
10) Отсортировать `fruits_and_vegatables.csv` по убыванию значения во столбца count
11) Найти названия во `fruits_and_vegatables.csv`, содержащие цифры
12) Извлечь IPv4-адреса из вывода `ip a`
