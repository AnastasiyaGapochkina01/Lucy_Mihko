1) Установить на ВМ php
2) В директории /opt создать папку rot13
3) В директории /opt/rot13 создать файл с именем rot13-server.php с кодом
```
<?php
$sock = socket_create(AF_INET, SOCK_DGRAM, SOL_UDP);
socket_bind($sock, '0.0.0.0', 10000);
for (;;) {
  socket_recvfrom($sock, $message, 1024, 0, $ip, $port);
  $reply = str_rot13($message);
  socket_sendto($sock, $reply, strlen($reply), 0, $ip, $port);
}
```
4) Запустить сервер командой php rot13-server.php и проверить что сервер работает: выполнить nc -u 127.0.0.1 10000 и ввести Hello World
5) Написать юнит-файл для запуска rot13-server.php как сервиса
6) Установить nginx, если он еще не установлен
7) Посмотреть статус демона nginx
8) Остановить nginx
9) Запустить nginx
10) Убрать nginx из автозагрузки
11) Вернуть nginx в автозагрузку
12) "Замаскировать" nginx, попробовать перезапустить/остановить/запустить nginx; снять с nginx "маскировку"
13) Установить redis ([install-redis-on-linux](https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/install-redis-on-linux/)) - ссылка доступна под VPN
14) Посмотреть статус демона redis
15) Запустить второй экземпляр демона redis, написав systemd unit
