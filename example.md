Создать
- пользователя `devuser` с домашней директорией `/opt/users/devuser` и комментарием "Development User"
- пользователя `testuser` с домашней директорией `/opt/users/testuser` и комментарием "QA User"
- пользователя `opstuser` с домашней директорией `/opt/users/opstuser` и комментарием "OPS User"
- группу `ecom` и добавить в нее всех созданных пользователей
```bash
sudo useradd -m -d /opt/users/devuser -c "Development User" devuser
sudo useradd -m -d /opt/users/testuser -c "QA User" testuser
sudo useradd -m -d /opt/users/opsuser -c "OPS User" opstuser
sudo usermod -aG ecom devuser sudo usermod -aG ecom testuser
sudo usermod -aG ecom opstuser
```
<img width="776" height="89" alt="image" src="https://github.com/user-attachments/assets/34bdb312-4432-45b2-849e-519b0807712d" />
