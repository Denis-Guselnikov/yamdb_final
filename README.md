# CI/CD проекта api_yamdb


### [Django-проект: api_yamdb](https://github.com/Denis-Guselnikov/api_yamdb):

REST API для проекта api_yamdb - база отзывов и рейтингов пользователей на произведения (книги, фильмы, музыка и пр.)

Сами произведения в api_yamdb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы и ставят произведению оценку в диапазоне от одного до десяти, из пользовательских оценок формируется усреднённая оценка произведения — рейтинг.

## CI и CD: Включает в себя 4 шага

![example branch parameter](https://github.com/Denis-Guselnikov/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

- Tests: автоматический запуск тестов
- Build: обновление образов на Docker Hub
- Deploy: автоматический деплой на боевой сервер при пуше в главную ветку main
- Inform: отправление сообщения в Telegram

## Для реализации проекта используются:

- Python 3.7.9
- Django 2.2.16
- Django REST Framework 3.12.4
- gunicorn 20.0.4
- psycopg2-binary
- docker
- docker-compose
- Ubuntu 20.04 LTS на сервере

## Структура проекта:
Такая структура должна быть перед запуском!

```
yamdb_final
├── .git/ 
├── .github/ 
│    └── workflows/
|          └── yamdb_workflow.yml <-- Workflow instruction
├── api_yamdb/
│    ├── api/
│    ├── api_yamdb/
│    │   ├── __init__.py
│    │   ├── settings.py
│    │   ├── urls.py
│    │   └── wsgi.py
│    ├── reviews/
│    ├── static 
│    │   └── redoc.yaml
│    ├── templates
│    │   └── redoc.html
|    ├── Dockerfile
│    ├── manage.py
|    └── requirements.txt 
├── infra/ <-- Deploy files
│    ├── nginx/ 
│    │   └── default.conf
│    ├── .env
|    └── docker-compose.yaml
├── tests/
├── .gitignore
├── pytest.ini
├── README.md
└── setup.cfg
```

## Подготовка репозитория

В settings/secrets нужно подготовить ключи:
```
- DOCKER_USERNAME - Username для DockerHub
- DOCKER_PASSWORD - Пароль для DockerHub
- HOST - хост или IP для deploy
- USER - Username на сервере
- SSH_KEY - Приватный ключ
- PASSPHRASE - Если ваш ssh-ключ защищён фразой-паролем
- DB_ENGINE - django.db.backends.postgresql_psycopg2
- DB_NAME - Имя базы данных
- POSTGRES_USER - Логин для подключения к базе данных 
- POSTGRES_PASSWORD - Пароль для подключения к БД
- DB_HOST - Название сервиса
- DB_PORT - порт для подключения к БД
```

## Запуск проекта

Клонировать репозиторий и перейти в него в командной строке:
```
git clone https://github.com/robky/yamdb_final.git
cd yamdb_final
```

Создать файл .env в infra/ и заполнить необходимыми данными:
```
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=db_name # имя базы данных
POSTGRES_USER=db_user # логин для подключения к базе данных
POSTGRES_PASSWORD=db_password # пароль для подключения к БД (установите свой)
DB_HOST=db_host # название сервиса (контейнера)
DB_PORT=5432  # порт для подключения к БД
SECRET_KEY=secret_key
```


### Когда сделаете git push - workflow начнёт работать

### Выполните по очереди команды:
Создать миграции:
```
docker-compose exec web python manage.py migrate
```
Создать суперпользователя
```
docker-compose exec web python manage.py createsuperuser
```
Статические данные:
```
docker-compose exec web python manage.py collectstatic --no-input
```

## Документация и админ-панель api_yamdb
```
Документация доступна по эндпойнту: http://158.160.46.110/redoc/
Админ-панель: http://158.160.46.110/admin/
```