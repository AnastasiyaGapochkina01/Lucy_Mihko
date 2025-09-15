1) Запустить скрипт в фоновом режиме (возможно потребуется дополнительно установить ПО)
```bash
#!/bin/bash

# Параметры
FILE="/tmp/testfile.lock"
PORT=8080
touch "$FILE"

run_holding_process() {
  (
    exec 3>"$FILE"
    while true; do
      nc -l "$PORT" >/dev/null 2>&1
    done
  ) &
  HOLD_PID=$!
  echo "Процесс занял порт и файл: PID $HOLD_PID"
}

cat << 'EOF' > zombie_creator.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid < 0) {
        perror("fork failed");
        exit(1);
    }

    if (pid == 0) {
        printf("Дочерний процесс: завершаюсь\n");
        _exit(0);
    } else {
        printf("Родительский процесс: живу 60 секунд, дочерний станет зомби\n");
        sleep(500);
        printf("Родительский процесс завершается\n");
        return 0;
    }
}

EOF

gcc zombie_creator.c -o zombie_creator
run_holding_process
./zombie_creator
```
2) Найти процесс, который "слушает" на порту 8080; найти какой файл "удерживает" этот процесс; убить этот процесс
3) Найти зомби-процесс; попробовать уничтожить (он будет жить 500 секунд после запуска скрипта, потом исчезнет сам)
4) Запустить скрипт в фоновом режиме (возможно потребуется создать пользователей)
```bash
#!/bin/bash

users=("appuser" "dbuser" "webuser")

commands=("sleep 2000" "sleep 3000" "sleep 4000")

nice_values=(19 5 0)

if [[ ${#users[@]} -ne ${#commands[@]} ]] || [[ ${#users[@]} -ne ${#nice_values[@]} ]]; then
  echo "Массивы users, commands и nice_values должны быть одинаковой длины"
  exit 1
fi

for i in "${!users[@]}"; do
  user="${users[$i]}"
  cmd="${commands[$i]}"
  nice_val="${nice_values[$i]}"

  if id "$user" &>/dev/null; then
    echo "Запуск команды '$cmd' от пользователя $user с nice=$nice_val"
    sudo -u "$user" nice -n "$nice_val" bash -c "$cmd" &
  else
    echo "Пользователь $user не существует, процесс не запущен! Создайте пользователя $user"
  fi
done

wait
```
5) Найти все процессы, запущенные этим скриптом, и выяснить их приоритет
6) Изменить приоритеты всем найденным в 5 задании процессам на 10
7) Запустить в фоновом режиме скрипт (возможно потребуется дополнительно установить ПО)
```bash
#!/bin/bash

if ! command -v cpulimit &> /dev/null; then
  echo "Ошибка: cpulimit не найден. Установите его и повторите."
  exit 1
fi

run_with_limits() {
  local cmd=$1
  local cpu_limit=$2   
  local file_limit=$3  
  local mem_limit=$4   

  echo "Запуск: '$cmd' с cpu_limit=${cpu_limit}%, file_limit=${file_limit}, mem_limit=${mem_limit}KB"

  ulimit -n "$file_limit"
  ulimit -v "$mem_limit"

  cpulimit -l "$cpu_limit" -- $cmd &
}

run_with_limits "yes > /dev/null" 10 1024 500000    # 10% CPU, 1024 open files, 500 МБ ВП
run_with_limits "yes > /dev/null" 50 2048 1000000   # 50% CPU, 2048 open files, 1 ГБ ВП
run_with_limits "yes > /dev/null" 80 4096 2000000   # 80% CPU, 4096 open files, 2 ГБ ВП

wait
echo "Все процессы завершены."
```
8) Найти все процессы, запущенные скриптом из задания 7; выяснить лимиты этих процессов
9) Убить все процессы из задания 8
