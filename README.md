# YAMDB_FINAL
![Yamdb-app_workflow](https://github.com/Galenfea/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

Предназначен для обмена мнениями о произведениях искусства и выставления им рейтинга, обеспечен CI:

- интерфейс реализован через REST API;
- есть регистрация и авторизация пользователей;
- добавление и удаление обзоров на произведения;
- выставление и изменение рейтинга;
- расчёт средней оценки произведения по обзорам;
- добавление, редактирование и удаление комментариев под обзорами;
- произведения разделены по категориями: кино, музыка, книги; и по жанрам;
- разделение ролей на пользователей, модераторов и администраторов;
- модераторы и администраторы могут добавлять произведения, новые жанры и новые категории;
- при push проводится тестирование на 

## Применяемые технологии:

- Python 3.7
- Django 2,2,16
- Django Rest Framework 3,12,4
- Docker 3.8
- Postgres 13.0
- Simple JWT 4,7,2
- Continuous Integration
- Continuous Deployment

## Как запустить проект:

**На Windows 10 корпоративной:**

***Если у вас не установлен Docker:***
- _откройте: Панель управления — Программы и компоненты — Включение и отключение компонентов Windows;_
- _активируйте пункт Hyper-V;_
- _перезагрузите систему._

_Установите Docker Desctop:_
[Docker Desctop для Windows](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe?utm_source=docker&utm_medium=webreferral&utm_campaign=dd-smartbutton&utm_location=header)

***Если у вас уже установлен Docker***

_Клонируйте репозиторий:_
```
git clone https://github.com/Galenfea/yamdb_final.git
```

_Перейдите в репозиторий в командной строке:_
```
cd yamdb_final/infra
```

_Создайте файл .env:_
```
touch .env
```

_Скопируйте в него следующий шаблон и установить собственные значения:_
```
DB_ENGINE=django.db.backends.postgresql 
DB_NAME=postgres 
POSTGRES_USER=postgres 
POSTGRES_PASSWORD=postgres 
DB_HOST=db 
DB_PORT=5432
WEB_SECRET_KEY='your_secret_key' # ваш секретный ключ
WEB_ALLOWED_HOSTS==['*']
```

_Соберите необходимые образы:_
```
docker-compose up -d
```

_Выполните миграции:_
```
winpty docker-compose exec web python manage.py migrate
```

_Соберите статику проекта в одном месте:_
```
 winpty docker-compose exec web python manage.py collectstatic --no-input
```

_Загрузите тестовые данные в базу:_
```
 winpty docker-compose exec web python manage.py loaddata fixtures.json
```

_Откройте админку:_
```
http://127.0.0.1/admin/
```

_Тестовый суперпользователь:_
```
username: admin
password: admin
```

## Документация
После запуска автономного сервера документация API расположена по ардесу:

http://127.0.0.1/redoc/


## Примеры запросов:

### Регистрация нового пользователя и получение токена:
**POST** /api/v1/auth/signup/  
_Request:_
```
{
    "email": "string",
    "username": "string"
}
```
_Response:_
```
{
    "email": "string",
    "username": "string"
}
```
**POST** /api/v1/auth/token/  
_Request:_
``` 
{
    "username": "string",
    "confirmation_code": "string"
}
```
_Response:_
```
{
    "token": "string"
}
```

### Получение списка всех произведений:
**GET** /api/v1/titles/

*Ответ в случае пустой базы:*
``` 
{
    "count": 0,
    "next": null,
    "previous": null,
    "results": []
}
``` 

*или вид ответа в случае наличия записей:*
``` 
{
    "count": 123,
    "next": "http://127.0.0.1/api/v1/titles/?offset=400&limit=100",
    "previous": "http://127.0.0.1/api/v1/titles/?offset=200&limit=100",
    "results": [
        {}
    ]
}
```

## Авторы:

- Козлов Павел -- система управления пользователями (Auth и Users): система регистрации и аутентификации, права доступа, работа с токеном, система подтверждения через e-mail;
- ***Отаров Александр*** -- упаковка в Docker, CI/CD, отзывы (Review) и комментарии (Comments): модели, представления, эндпойнты для них, рейтинги произведений; 
- Черваков Дмитрий -- категории (Categories), жанры (Genres) и произведения (Titles): модели, представления и эндпойнты для них.

## Адрес сервера для деплоя в рамках CI/CD:

http://158.160.10.40/redoc/ - документация по API

http://158.160.10.40/admin/ - вход в админку

_Тестовый суперпользователь:_
```
username: admin
password: admin
```
tet