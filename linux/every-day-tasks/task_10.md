# CMDB
Для предыдущего проекта подрядчик написал скрипт нагрузочного тестирования. Код скрипта ниже
<details>
  <summary>load-tests.sh</summary>
  
```bash
#!/bin/bash

BASE_URL="http://localhost:3000"
RESULTS_DIR="./load-test-results"
RESULTS_FILE="${RESULTS_DIR}/load-test-results.txt"
RPS=20                    # Запросов в секунду
DURATION=30               # Секунд
TIMEOUT=5                 # Таймаут запроса

GET_VALID_WEIGHT=30       # 30% - корректный GET /api/servers
POST_VALID_WEIGHT=20      # 20% - корректный POST /api/servers
INVALID_PATH_WEIGHT=20    # 20% - несуществующий путь
INVALID_METHOD_WEIGHT=30  # 30% - неверные методы

SERVER1='{"hostname":"web01","ip_address":"192.168.1.10","role":"nginx"}'
SERVER2='{"hostname":"db01","ip_address":"192.168.1.11","role":"mysql"}'
SERVER3='{"hostname":"app01","ip_address":"192.168.1.12","role":"php-fpm"}'
SERVER4='{"hostname":"cache01","ip_address":"192.168.1.13","role":"redis"}'

SERVERS=("$SERVER1" "$SERVER2" "$SERVER3" "$SERVER4")

mkdir -p "$RESULTS_DIR"
> "$RESULTS_FILE"

TOTAL=0
SUCCESS=0
ERRORS=0
START_TIME=$(date +%s)

echo "🚀 Нагрузочный тест: ${RPS} req/s на ${DURATION}с"
echo "Распределение: GET:${GET_VALID_WEIGHT}% POST:${POST_VALID_WEIGHT}% INVALID:${INVALID_PATH_WEIGHT}% METHODS:${INVALID_METHOD_WEIGHT}%"
echo

log_result() {
  local type=$1 req_num=$2 status=$3 response_time=$4 error=$5 endpoint=$6 method=$7
  local timestamp=$(date -Iseconds)
  local log_entry="${timestamp} | #${req_num} | ${type} | ${status} | ${response_time}ms | ${error:-OK} | ${endpoint} | ${method}"

  echo "$log_entry" >> "$RESULTS_FILE"

  if [[ "$status" =~ ^(4|5) ]] || [ -n "$error" ]; then
    ((ERRORS++))
  else
    ((SUCCESS++))
  fi

  ((TOTAL++))
}

send_get_valid() {
  local req_num=$1
  local start_time=$(date +%s%N)

  if response=$(curl -s -w "%{http_code}" -o /dev/null -m "$TIMEOUT" "$BASE_URL/api/servers"); then
    local status="${response: -3}"
    local response_time=$(( ($(date +%s%N) - start_time) / 1000000 ))
    log_result "GET" "$req_num" "$status" "$response_time" "" "/api/servers" "GET"
  else
    local response_time=$(( ($(date +%s%N) - start_time) / 1000000 ))
    log_result "GET" "$req_num" "TIMEOUT" "$response_time" "timeout" "/api/servers" "GET"
  fi
}

send_post_valid() {
  local req_num=$1
  local server_data=${SERVERS[$((RANDOM % ${#SERVERS[@]}))]}
  local start_time=$(date +%s%N)

  if response=$(curl -s -w "%{http_code}" -o /dev/null -m "$TIMEOUT" -H "Content-Type: application/json" \
    -d "$server_data" "$BASE_URL/api/servers"); then
    local status="${response: -3}"
    local response_time=$(( ($(date +%s%N) - start_time) / 1000000 ))
    log_result "POST" "$req_num" "$status" "$response_time" "" "/api/servers" "POST"
  else
    local response_time=$(( ($(date +%s%N) - start_time) / 1000000 ))
    log_result "POST" "$req_num" "TIMEOUT" "$response_time" "timeout" "/api/servers" "POST"
  fi
}

send_invalid_path() {
  local req_num=$1
  local paths=("/api/nonexistent" "/api/servers/999/delete" "/health" "/admin" "/favicon.ico")
  local invalid_path=${paths[$((RANDOM % ${#paths[@]}))]}
  local start_time=$(date +%s%N)

  if response=$(curl -s -w "%{http_code}" -o /dev/null -m "$TIMEOUT" "$BASE_URL$invalid_path"); then
    local status="${response: -3}"
    local response_time=$(( ($(date +%s%N) - start_time) / 1000000 ))
    log_result "INVALID_PATH" "$req_num" "$status" "$response_time" "" "$invalid_path" "GET"
  else
    local response_time=$(( ($(date +%s%N) - start_time) / 1000000 ))
    log_result "INVALID_PATH" "$req_num" "TIMEOUT" "$response_time" "timeout" "$invalid_path" "GET"
  fi
}

send_invalid_method() {
  local req_num=$1
  local methods=("PUT" "DELETE" "PATCH" "HEAD" "OPTIONS")
  local method=${methods[$((RANDOM % ${#methods[@]}))]}
  local start_time=$(date +%s%N)

  if response=$(curl -s -w "%{http_code}" -X "$method" -o /dev/null -m "$TIMEOUT" "$BASE_URL/api/servers"); then
    local status="${response: -3}"
    local response_time=$(( ($(date +%s%N) - start_time) / 1000000 ))
    log_result "INVALID_METHOD" "$req_num" "$status" "$response_time" "" "/api/servers" "$method"
  else
    local response_time=$(( ($(date +%s%N) - start_time) / 1000000 ))
    log_result "INVALID_METHOD" "$req_num" "TIMEOUT" "$response_time" "timeout" "/api/servers" "$method"
  fi
}

end_time=$((START_TIME + DURATION))
req_num=0

while [ $(date +%s) -lt "$end_time" ]; do
  current_time=$(date +%s)
  elapsed=$((current_time - START_TIME))
  progress=$(( (elapsed * 100) / DURATION ))

  printf "\r📊 Прогресс: %d%% (%d/%ds) | Запросов: %d" "$progress" "$elapsed" "$DURATION" "$TOTAL"

  for ((i=0; i<RPS; i++)); do
    ((req_num++))
    rand=$((RANDOM % 100))

    if [ $rand -lt $GET_VALID_WEIGHT ]; then
      send_get_valid "$req_num" &
    elif [ $rand -lt $((GET_VALID_WEIGHT + POST_VALID_WEIGHT)) ]; then
      send_post_valid "$req_num" &
    elif [ $rand -lt $((GET_VALID_WEIGHT + POST_VALID_WEIGHT + INVALID_PATH_WEIGHT)) ]; then
      send_invalid_path "$req_num" &
    else
      send_invalid_method "$req_num" &
    fi
  done
  sleep 1
done

wait
printf "\r📊 Завершение...           \n"

DURATION_SEC=$(( $(date +%s) - START_TIME ))
RPS_REAL=$(( TOTAL / DURATION_SEC ))
SUCCESS_RATE=$(( (SUCCESS * 100 + TOTAL - 1) / TOTAL ))  # Округление вверх

cat << EOF

📊 ИТОГИ ТЕСТА (${DURATION_SEC}s):
  ✅ Всего запросов: ${TOTAL}
  ✅ Успешных (2xx): ${SUCCESS}
  ❌ Ошибок (4xx/5xx): ${ERRORS}
  📈 Успешность: ${SUCCESS_RATE}%
  ⚡ RPS: ${RPS_REAL}

📋 Статистика:
EOF

awk '
/GET \/api\/servers GET/ && $4 == 200 {get_ok++}
/POST \/api\/servers POST/ && $4 == 201 {post_ok++}
/INVALID_PATH/ && $4 == 404 {path_404++}
/INVALID_METHOD/ && $4 == 405 {method_405++}
END {
  print "  📋 GET /api/servers (200): " get_ok
  print "  📋 POST /api/servers (201): " post_ok
  print "  📋 404 Not Found: " path_404
  print "  📋 405 Method Not Allowed: " method_405
  print "  📋 Результаты: '"${RESULTS_FILE}"'"
}' "$RESULTS_FILE"
```
</details>
Нужно натравить скрипт на запущенный  https://gitlab.com/devops201206/node-cmdb_with_logs и из результатов тестирования получить

- количество успешных POST запросов
- количество ошибок INVALID_PATH
- url, на которых была получена ошибка INVALID_PATH

Дополнительно нужно вывести в файле nodeapp.log убрать в начале каждой строки `::ffff:` и вывести в файл nodeapp_clean.log
<img width="1273" height="210" alt="image" src="https://github.com/user-attachments/assets/46021db4-7b8c-4af3-82d8-fd751d69a479" />
