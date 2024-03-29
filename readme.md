# Binance api
> Важное замечание: Сервис кеширует результаты запросов к binance api в хранилище Redis, время жизни записей конфигурируется в файле .env, по умолчанию 3 секунды. При этом PostgreSQL подключен к проекту для теоритического расширения функционала.
## Fast setup
Предварительно нужно создать файл ```.env``` в корне проекта по шаблону .env.example

Далее надо освободить порт 5432, используемый обычно Postgres, после чего запускаем контейнер:

```shell
docker network create binance_api
docker-compose up -d --build
docker compose exec binance_api migrate
```

## Commands
### Migrations
- Create an automatic migration from changes in `src/models.py`
```shell
docker compose exec binance_api makemigrations users_added # or other name of the migration
```

- Run migrations
```shell
docker compose exec binance_api migrate
```
- Downgrade migrations
```shell
docker compose exec binance_api downgrade -1  # or -2 or base or hash of the migration
```

### Linter
```shell
docker compose exec binance_api format
```

### Tests
```shell
docker compose exec binance_api pytest
```

### Redis connection
```shell
docker exec -it redis sh
```

```shell
redis-cli -h redis_db -p 6379 -a myStrongPassword
```
------
## Работа с запущенным сервером
По умолчанию сервер доступен на локальной сети на порту 8000
http://127.0.0.1:8000/
> Для просмотра документации допишите в конце адреса путь docs:
> http://127.0.0.1:8000/docs

## Production deploy
В файл .env добавить переменную SENTRY_DSN для мониторинга и заменить ENVIRONMENT. Например: 

```
...
ENVIRONMENT=PRODUCTION

SENTRY_DSN=https://123456789.ingest.sentry.io/987654321
...
```

Запустить контейнер
```shell
docker network create binance_api
docker-compose -f docker-compose.prod.yml up -d --build
```


