# Продолжение про установку пакетов
https://youtu.be/2RrmMs0FP18

# Установка ПО
1) Установить на ВМ apache2
2) Установить на ВМ docker по официальной инструкции
3) Удалить apache2 с удалением всех зависимостей и конфигов
4) Скачать пакет cowsay отсюда https://packages.debian.org/sid/all/cowsay/download
```bash
wget http://ftp.de.debian.org/debian/pool/main/c/cowsay/cowsay_3.03+dfsg2-8_all.deb
```
и установить его. Затем попробовать добавить в конец файла `~/.bashrc` строки
```bash
if [ -x /usr/games/cowsay -a -x /usr/games/fortune ]; then
    fortune | cowsay
fi
```
и перезайти по ssh на сервер. Должно получиться что-то такое

<img width="717" height="275" alt="image" src="https://github.com/user-attachments/assets/8b9bc80f-0e62-4507-abb9-c3ba2d0b53a3" />

5) Узнать, установлен ли в системе пакет curl
6) Установить пакет build-essential
7) Добавить официальный репозитори Node.js v22 для установки актуальной версии
8) Скачать иходный код tmux
```bash
wget https://github.com/tmux/tmux/archive/refs/tags/3.5a.tar.gz
```
далее
- распаковать скачанный архив
- перейти в распакованную директорию и проверить наличие всех необходимых зависимостей и сгенерировать Makefile
- запустить компиляцию программы
- установить скомпилированные бинарные файлы
- проверить что tmux работает (https://linuxize.com/post/getting-started-with-tmux/)

9) Освободить место на диске, удалив закешированные .deb-файлы
# Работа с текстом
1) Создать файлы
- access.log
```
192.168.1.1 - - [10/Oct/2023:12:34:56 +0000] "GET /api/user HTTP/1.1" 200 1234
192.168.1.2 - - [10/Oct/2023:12:34:57 +0000] "GET /static/css/style.css HTTP/1.1" 200 5432
192.168.1.1 - - [10/Oct/2023:12:35:01 +0000] "POST /api/user HTTP/1.1" 201 42
192.168.1.3 - - [10/Oct/2023:12:35:04 +0000] "GET /api/products HTTP/1.1" 500 0
192.168.1.2 - - [10/Oct/2023:12:35:05 +0000] "GET / HTTP/1.1" 200 9876
192.168.1.4 - - [10/Oct/2023:12:35:06 +0000] "GET /images/logo.png HTTP/1.1" 200 2048
192.168.1.5 - - [10/Oct/2023:12:35:07 +0000] "POST /api/cart HTTP/1.1" 201 128
192.168.1.1 - - [10/Oct/2023:12:35:10 +0000] "GET /api/user/123 HTTP/1.1" 200 764
192.168.1.3 - - [10/Oct/2023:12:35:12 +0000] "GET /api/products/45 HTTP/1.1" 404 0
192.168.1.2 - - [10/Oct/2023:12:35:15 +0000] "GET /static/js/app.js HTTP/1.1" 200 6578
192.168.1.4 - - [10/Oct/2023:12:35:16 +0000] "DELETE /api/cart/321 HTTP/1.1" 204 0
192.168.1.5 - - [10/Oct/2023:12:35:18 +0000] "POST /api/login HTTP/1.1" 200 256
192.168.1.1 - - [10/Oct/2023:12:35:19 +0000] "GET /dashboard HTTP/1.1" 302 123
192.168.1.3 - - [10/Oct/2023:12:35:21 +0000] "GET /api/orders HTTP/1.1" 500 0
192.168.1.2 - - [10/Oct/2023:12:35:22 +0000] "GET /favicon.ico HTTP/1.1" 200 543
```
- config.json
```json
{
  "app_name": "ecom-web",
  "version": "2.8.1",
  "database": {
    "host": "db-primary.prod.tech.com",
    "port": 5432,
    "name": "ecom_web"
  },
  "logging": {
    "level": "error",
    "file_path": "/var/log/ecom_web.log"
  }
}
```
- services.csv
```csv
service_name,version,status,cluster
api-gateway,1.4.0,running,production
user-service,2.1.3,running,staging
auth-service,1.0.5,stopped,production
balancer,1.24,stopped,dev
database,10.4.0,running,production
payment-service,2.8.1,running,production
balancer,1.23,running,production
database,5.7,running,dev
```
2) Найти все успешные запросы в `access.log`
3) Узнать, сколько раз IP-адрес `192.168.1.1` обращался к серверу и какие uri он запрашивал
4) Получить список всех уникальных IP-адресов из `access.log`
5) Вывести все строки из `services.csv`, где статус НЕ "running".
6) Найти все dev сервисы в файле `services.csv`
7) Из файла `app_config.json` вывести только значение поля version
8) Из файла `app_config.json` вывести только значение поля file_path
9) Создать файл `gen_ecom_logs.sh`
```bash
#!/bin/bash

LOG_FILE="/var/log/ecom_web.log"

HOSTNAMES=("web01" "web02" "web03")
ENVS=("dev" "staging" "prod")
MESSAGES=(
    "Started processing request"
    "Database connection established"
    "User login successful"
    "User login failed"
    "Payment transaction completed"
    "Payment transaction failed"
    "Cache refreshed"
    "API response sent"
    "Background job started"
    "Background job finished"
)

LINES=50

> "$LOG_FILE"

for i in $(seq 1 $LINES); do
    TIMESTAMP=$(date +"%Y-%m-%d %H:%M:%S")

    HOST=${HOSTNAMES[$RANDOM % ${#HOSTNAMES[@]}]}
    ENV=${ENVS[$RANDOM % ${#ENVS[@]}]}
    MSG=${MESSAGES[$RANDOM % ${#MESSAGES[@]}]}

    echo "[$TIMESTAMP] $HOST $ENV $MSG" >> "$LOG_FILE"
    sleep 1
done

echo "Лог сгенерирован: $LOG_FILE"
```
10) Запустить созданный `gen_ecom_logs.sh` командой
```bash
sudo bash gen_ecom_logs.sh > /dev/null 2>&1 &
```
11) В реальном времени следить за тем, что добавляется в конец файла `/var/log/ecom_web.log`
12) Найти все файлы в директории `/var/log` размером больше 100 мегабайт (таких может и не быть)
13) Из вывода команды `timedatectl status` получить значение `Time zone`
14) Из вывода команды `ip link show` получить только названия интерфейсов (у тебя могут быть другие имена)

<img width="871" height="180" alt="ip" src="https://github.com/user-attachments/assets/cd37ff1d-6d93-4cde-bf82-07e702f16105" />

# Работа с пользователями и правами доступа
0) Изучить механизм sudoers в Linux
1) Создать пользователя jenkins; задать пароль для только что созданного пользователя
2) Создать пользователя logger и сразу создать для него домашнюю директорию /var/logger
3) Создать пользователя gitlab-runner
4) Создать группу deployers, добавить в нее пользователей jenkins и gitlab-runner
5) Дать возможность группе deployers выполнять команды `docker run, docker stop, docker start, docker rm, docker ps` без пароля
6) Сменить владельца файла `/var/log/ecom_web.log` на logger
7) Сменить группу-владельца файла `/var/log/ecom_web.log` на deployers
8) Создать файл `gen_go_project.sh`
```bash
#!/bin/bash

PROJECT=$1
if [ -z "$PROJECT" ]; then
  echo "Usage: $0 <project-name>"
  exit 1
fi
mkdir -p $PROJECT/{cmd/$PROJECT,internal,pkg}
cd $PROJECT
echo "# $PROJECT" > README.md
echo -e "bin/\n*.exe\n*.log\nvendor/" > .gitignore

cat <<EOF > cmd/$PROJECT/main.go
package main

import "fmt"

func main() {
    fmt.Println("Hello, $PROJECT!")
}
EOF

echo "Project files generated successfully."
```
9) Сделать файл `gen_go_project.sh` исполняемым
10) Выполнить `bash gen_go_project.sh ecommerce`
11) Посмотреть, создалась ли рядом с скриптом директория `ecommerce`; вывести ее содержимое в виде дерева

<img width="454" height="207" alt="tree" src="https://github.com/user-attachments/assets/d751fc5f-d504-4a82-b20d-4c742ddb07b8" />

12) Изменить владельца и группу для всей директории `ecommerce` на `root:deployers`
13) Для директории `ecommerce` и всех файлов внутри установить права 774
14) Посмотреть UID, GID и группы пользователя jenkins
15) Создать системного пользователя service_account без создания домашней директории
