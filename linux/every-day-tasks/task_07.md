# Side project
Тебе предложили небольшую подработку DevOps на part-time в очередном крипто-стартапе. Есть проект
https://gitlab.com/scout8285648/crypto.tech

Проект уже докеризован
<details>
  <summary>Dockerfile</summary>
  
```
FROM python:3.12-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .
COPY app /app/app
```
</details>

<details>
  <summary>compose.yaml</summary>

```yaml
services:
  postgres:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: crypto
      POSTGRES_USER: crypto
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U crypto"]
      interval: 10s
      timeout: 5s
      retries: 5

  app:
    build: .
    environment:
      - DATABASE_URL=postgresql://crypto:password@postgres:5432/crypto
      - SECRET_KEY=your-super-secret-key-change-in-production
      - FLASK_APP=app:create_app()
      - FLASK_ENV=development
    depends_on:
      postgres:
        condition: service_healthy
    volumes:
      - ./static:/app/app/static
      - ./templates:/app/app/templates
      - .:/app

  nginx:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./static:/usr/share/nginx/html/static:ro
      - ./nginx/crypto.conf:/etc/nginx/conf.d/default.conf:ro
    depends_on:
      - app

volumes:
  postgres_data:
```
</details>
Но есть проблемка - **нужно автоматизировать применение миграций через bash-скрипт**

> [!TIP]
> Миграции это вот это
> ```
> docker compose up -d postgres
> docker compose run --rm app flask db init
> docker compose run --rm app flask db migrate -m "Initial migration"
> docker compose run --rm app flask db upgrade
> ```
