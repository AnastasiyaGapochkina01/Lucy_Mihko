# Материалы
## Linux

---

1) [Работа с systemd](https://youtu.be/fmVI9Q2LavI)
    - [домашка](https://github.com/AnastasiyaGapochkina01/Lucy_Mihko/blob/main/home_works/systemd.md)
2) [cron](https://youtu.be/hTkaCE5Mz8I)
    - [домашка](https://github.com/AnastasiyaGapochkina01/Lucy_Mihko/blob/main/home_works/cron.md)
3) [Работа с дисками, LVM](https://youtu.be/DnUqaXJVdew)
    - [домашка](https://github.com/AnastasiyaGapochkina01/Lucy_Mihko/blob/main/home_works/lvm.md)
4) [RAID](https://youtu.be/yDqkN2ecNEU)
     - [домашка raid](https://github.com/AnastasiyaGapochkina01/Lucy_Mihko/blob/main/home_works/raid.md)
5) [Сборка `deb` пакета](https://youtu.be/C29lccc2R5w)
    - [домашка](https://github.com/AnastasiyaGapochkina01/Lucy_Mihko/blob/main/home_works/deb.md)
6) Сети
- https://youtu.be/3zCQGen6VpA
- https://youtu.be/2OljPJn3H1Q
- https://youtu.be/abmnnC4kZnI
- https://youtu.be/Bd9itUD1_aU
7) [Bash-скриптинг](https://youtu.be/kEpCiCb-y1Y)
    - [домашка](https://github.com/AnastasiyaGapochkina01/Lucy_Mihko/blob/main/home_works/bash.md)

## Docker

---

1) [Введение](https://youtu.be/2LKrD8VRyg4)
2) [Images](https://youtu.be/nKnmoJauQKw)
3) [Registry](https://youtu.be/nKnmoJauQKw)
4) [Multistage, compose](https://youtu.be/W7ku22XUFUc)
5) [Compose part 2](https://youtu.be/ef-Kmw4FShU)
6) [Namespaces и Cgroups (опиционально к изучению)](https://youtu.be/dsjEUP6OV_w)

### [Домашка](https://github.com/AnastasiyaGapochkina01/Lucy_Mihko/blob/main/home_works/docker.md)

## CI-CD

---

1) [Начало работы с gitlab-ci](https://youtu.be/cbStvZv2fMg)
2) [Простые пайплайны](https://youtu.be/c4Mdb-zJRy8)
3) [Работа с артефактами](https://youtu.be/x1ZiiobQbmg)

### [Домашка](https://github.com/AnastasiyaGapochkina01/Lucy_Mihko/blob/main/home_works/gitlab-ci.md)

## Ansible

---

1) [Введение, ad-hoc, inventory, playbooks](https://youtu.be/xYYWsLQk8bY)
2) [Переменные, include](https://youtu.be/Af5sGecqbU4)
3) [Роли](https://youtu.be/qP8lJF_bWWo)
4) [Тестирование ролей с помощью molecule](https://youtu.be/fOyWevTYBZA)

### [Домашка](https://github.com/AnastasiyaGapochkina01/Lucy_Mihko/blob/main/home_works/ansible.md)

## Terraform
1) [Начало работы с YCloud](https://youtu.be/cTMvb-0kibA)
2) [Начало работы с terraform](https://youtu.be/AwTc65TOjpQ)

### [Домашка](https://github.com/AnastasiyaGapochkina01/Lucy_Mihko/blob/main/home_works/terraform.md)

---

# Мини-проект
Необходимо создать pipeline, который будет:
- Собирать Docker-образ приложения https://github.com/AnastasiyaGapochkina01/go-http-db
- Развертывать приложение на удаленном сервере с помощью Ansible
- Проверять работоспособность развернутого приложения
### Требования к реализации
1) Docker:
  - делать multistage сборку
  - публиковать собранный image в container registry
  - у БД должен быть healthcheck
  - деплой осуществлять с помощью docker compose, который должен содержать сервисы
    - приложение
    - postgres
    - migrator
    ```
    migrator:
      image: postgres:15
      restart: on-failure
      environment:
        PGPASSWORD: postgres
      volumes:
        - ./migrations:/migrations
      command: >
        sh -c "while ! pg_isready -h db -U postgres; do sleep 1; done && psql -h db -U postgres -d app_db -f /migrations/001_init.sql"
    ```
2) Ansible:
- написать роли для
  - установки docker
  - разворачивания приложения
3) GitLab CI:
- pipeline должен содержать этапы
  - linter (для файлов приложения, файлов docker, файлов ansible)
  - build
  - test (запуск всей инфраструктуры любым способом и простой запрос ```curl http://$host:8080/hello```)
  - deploy
- должно быть ручное подтверждение перед деплоем
