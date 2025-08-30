# Установка ПО
1) Проверить, установлены ли на ВМ пакеты python3-pip и python3-venv. Если нет, то установить их
2) Установить пакет pandoc на ВМ (для debian можно взять отсюда https://packages.debian.org/search?keywords=pandoc); проверить установку
```bash
pandoc --version
pandoc -f markdown -t html <(echo "# Hello")
# результат
<h1 id="hello">Hello</h1>
```
3) Установить `mc` из исходников (https://midnight-commander.org/source-code/)
4) В файле `/var/log/apt/history.log` найти последние 5 установленных пакета (нужны только названия пакетов) и удалить последний пакет, который был установлен
5) Очистить систему от неиспользуемых зависимостей
6) Скомпилировать с помощью CMake код на C++ (подсказка тут https://labex.io/tutorials/linux-linux-cmake-command-with-practical-examples-422608) 
##### main.cpp
```cpp
#include <iostream>
#include <chrono>
#include <ctime>
#include <unistd.h>
#include <sys/utsname.h>

int main() {
    struct utsname os_info;
    if (uname(&os_info) != 0) {
        std::cerr << "Ошибка получения информации об ОС" << std::endl;
        return 1;
    }

    auto now = std::chrono::system_clock::now();
    std::time_t current_time = std::chrono::system_clock::to_time_t(now);
    char time_str[100];
    std::strftime(time_str, sizeof(time_str), "%Y-%m-%d %H:%M:%S", std::localtime(&current_time));

    char hostname[256];
    if (gethostname(hostname, sizeof(hostname)) != 0) {
        std::cerr << "Ошибка получения имени хоста" << std::endl;
        return 1;
    }

    std::cout << "OS: " << os_info.sysname << " " << os_info.release
              << " (" << os_info.version << ")" << std::endl;
    std::cout << time_str << " Hello from " << hostname << std::endl;

    return 0;
}
```

при запуске исполняемого файла должно получиться такое
<img width="741" height="84" alt="image" src="https://github.com/user-attachments/assets/79f9e163-be5e-40b0-beca-cd163350df3d" />

# Работа с текстом
1) Подсчитать количество строк в файле /var/log/syslog
2) Посчитать количество вхождений слова "error" в файле /var/log/syslog (без учета регистра)
3) Извлечь все _уникальные_ ip-адреса из файла `/var/log/auth.log`
4) Преобразовать вывод команды 
`
echo | openssl s_client -connect "ya.ru:443" -servername "ya.ru" 2>/dev/null | openssl x509 -noout -enddate | cut -d= -f2
`
в формат `YYYY-MM-DD`
5) Из вывода команды `ls -l` извлечь только права доступа и названия файлов/директорий
6) В выводе команды `journalctl -xe` найти все fail или error записи
7) Создать файл `data.csv`
```csv
timestamp,ip,status,method,url
2023-10-05T12:30:45,192.168.1.1,200,GET,/index.html
2023-10-05T12:31:22,192.168.1.15,404,GET,/nonexistent.html
2023-10-05T12:32:01,192.168.1.7,200,POST,/login.php
2023-10-05T12:32:45,192.168.1.99,500,GET,/api/data
2023-10-05T12:33:10,192.168.1.1,200,GET,/styles.css
2023-10-05T12:33:22,192.168.1.15,404,GET,/favicon.ico
2023-10-05T12:34:05,192.168.1.7,200,GET,/dashboard
2023-10-05T12:34:18,192.168.1.3,403,GET,/admin
```
8) Разделить файл `data.csv` на два файла: `success.csv` (строки с "200") и `errors.csv` (строки с "404")
9) Найти все файлы больше 100 МБ в директориях `/var/log` и `/etc` и сохранить их список в `large_files.txt` в формате
```text
/var/lib/docker/containers/abcd1234/aabbbcccc.log	1.2G
/home/user/Videos/vacation.mp4	950M
/opt/app/database.sqlite	890M
```
10) Для команды `ip -4 -o a` вывести только названия и ip-адреса интерфейсов
<img width="368" height="100" alt="image" src="https://github.com/user-attachments/assets/2d33394f-2015-415f-b659-ac40e3c6f8c8" />

# Пользователи и права доступа
### Изучить ACL в Linux
0) Удалить всех пользователей и группы из предыдущей домашки (тех что были созданы самостоятельно командами `groupadd` и `adduser/useradd`)
1) Создать пользователей
- zabbix (без возможности интерактивного входа)
- node-exporter
- ansible
- tfuser
- appmonitor
2) Создать группы
- monitroing
- infra
3) Добавить в группу monitroing пользователей zabbix, node-exporter, appmonitor; в группу infra - ansible и tfuser
4) Создать директорию `/shared-dir` с правами доступа `775`, где владельцем является `root`, а групой-владельцем - `infra`
5) Настроить sudo для группы infra так, чтобы она могла выполнять без пароля команды
```bash
systemctl status nginx
systemctl stop nginx
systemctl start nginx
```
6) Настроить sudo для пользователя ansible так, чтобы он мог выполнять все команды без пароля
7) Для пользователя tfuser установить постоянный umask 002
8) Создать директорию `/opt/terraform` и дать права на запись в нее только tfuser
9) Создать директорию `/var/log/app` и назначить владельцем и группой-владельцем appmonitor
10) Разрешить пользователю node-exporter читать системные метрики (это права на чтение директорий `/proc` и `/sys`)
11) Разрешить пользователю zabbix читать логи из `/var/log/app`, но не изменять их (не меняя владельца/группу)
12) Удалить директорию `/opt/terraform`; создать директории `/opt/infrastructure/ansible` и `/opt/infrastructure/terraform`
13) Настроить права на `/opt/infrastructure/ansible` так, чтобы
- ansible имел полные права
- tfuser имел права на чтение и выполнение
14) Настроить права на `/opt/infrastructure/terraform` так, чтобы
- tfuser имел полные права
- ansible имел права на чтение и выполнение
