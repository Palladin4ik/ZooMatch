# Запуск проекта

1. Клонируйте репозиторий
```bash
git clone https://github.com/Palladin4ik/ZooMatch.git
cd ZooMatch/ZooMatch
```
2. Создайте файл переменных окружения
Скопировать пример и заполнить значения:

```bash
cp .env.example .env.docker
```

Открыть `.env.docker` и заполнить:

```dotenv
# База данных
POSTGRES_DB=zoomatch
POSTGRES_USER=zoomatch_user
POSTGRES_PASSWORD=your_password
DB_HOST=db
DB_PORT=5432

# Django
SECRET_KEY=your_secret_key
DEBUG=True
ALLOWED_HOSTS=localhost,127.0.0.1
CORS_ALLOWED_ORIGINS=http://localhost:5173,http://localhost:3000

# Redis
REDIS_URL=redis://redis:6379/0
CELERY_BROKER_URL=redis://redis:6379/1
CELERY_RESULT_BACKEND=redis://redis:6379/1

# Yandex Maps
YANDEX_MAPS_API_KEY=your_yandex_maps_api_key
```

> **Важно:** `DB_HOST=db` и `redis://redis:...` — внутри Docker-сети сервисы обращаются друг к другу по именам, не по `localhost`.

3. Запустите контейнеры
```bash
docker compose up -d
```

4. Примените миграции

```bash
docker compose exec backend python manage.py migrate
```
5. Загрузить фикстуры

```bash
docker compose exec backend python manage.py loaddata fixtures/action_categories
docker compose exec backend python manage.py loaddata fixtures/test_data
```

6. Собрать статику

```bash
docker compose exec backend python manage.py collectstatic --no-input
```


## Запуск фронтенда (Vue 3)

```bash
cd ../zoomatch-web
```

Создать файл `.env.local`:

```dotenv
VITE_API_URL=http://localhost:8000
VITE_WS_URL=ws://localhost:8000
VITE_MEDIA_URL=http://localhost:8000
VITE_YANDEX_MAPS_API_KEY=your_yandex_maps_api_key
```

Установить зависимости и запустить:

```bash
npm install
npm run dev
```

Фронтенд будет доступен по адресу: http://localhost:5173


## Документация
API документация доступна по адресам
http://127.0.0.1:8000/api/docs/redoc/ или http://127.0.0.1:8000/api/docs/swagger/
