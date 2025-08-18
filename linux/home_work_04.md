1) Создать пользователя devops с домашней директорией и комментарием "DevOps Engineer"; установить пароль qwerty123 для пользователя devops
2) Создать группу docker и добавить в неё пользователя devops
3) Проверить принадлежность пользователя devops к группе docker
4) Установить zsh (https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH)
5) Изменить основной shell для devops на `/bin/zsh`
6) Создать директорию `/opt/srv/backups` с владельцем `devops:docker` и правами `770`
7) Создать директорию `/opt/srv/nginx`, установить владельца `root:www-data` и права `750`
8) Создать файлы
- `/opt/srv/nginx/nginx.conf`
- `/opt/srv/nginx/mime.types`
- `/opt/srv/nginx/backend.conf`
9) Рекурсивно изменить владельца всех файлов в `/opt/srv/nginx` на `www-data:devops` и права на `644`
10) Изменить UID пользователя `devops` на `1500`
11) Создать пользователя `gitlab-runner` с системным UID (без домашней директории)
