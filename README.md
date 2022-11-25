# yamdb_final
yamdb_final


![API_YAMDB_FINAL](https://github.com/authorisright/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)




# API YaMDb

Проект YaMDb собирает отзывы пользователей на произведения. Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
Произведения делятся на категории, такие как «Книги», «Фильмы», «Музыка».
Произведению может быть присвоен жанр из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»).
Добавлять произведения, категории и жанры может только администратор.
Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.
Добавлять отзывы, комментарии и ставить оценки могут только аутентифицированные пользователи.

  

## Ресурсы API YaMDb
* Ресурс auth: аутентификация.
* Ресурс users: пользователи.
* Ресурс titles: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).
* Ресурс categories: категории (типы) произведений («Фильмы», «Книги», «Музыка»).
* Ресурс genres: жанры произведений. Одно произведение может быть привязано к нескольким жанрам.
* Ресурс reviews: отзывы на произведения. Отзыв привязан к определённому произведению.
* Ресурс comments: комментарии к отзывам. Комментарий привязан к определённому отзыву.

Каждый ресурс описан в документации: указаны эндпоинты (адреса, по которым можно сделать запрос), разрешённые типы запросов, права доступа и дополнительные параметры, если это необходимо.


После запуска проекта, по адресу http://178.154.222.150//redoc/ будет доступна документация для Yamdb API.

## Как запустить проект:

1. Клонировать репозиторий и перейти в него в командной строке:

 ```
git clone https://github.com/authorIsRight/yamdb_final.git 

```
Затем нужно перейти в папку yamdb_final/infra и создать в ней файл .env с переменными окружения, необходимыми для работы приложения.

```
cd infra/
```

Пример содержимого файла:

```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
```

Далее следует запустить docker-compose:

```
docker-compose up -d

```

Будут созданы и запущены в фоновом режиме необходимые для работы приложения контейнеры (db, web, nginx).

Затем нужно внутри контейнера web выполнить миграции, создать суперпользователя и собрать статику:

```
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input 
```

После этого проект должен быть доступен по адресу  [http://localhost/](http://localhost/).

### Заполнение базы данных

Нужно зайти на на  [http://localhost/admin/](http://localhost/admin/), авторизоваться и внести записи в базу данных через админку.

Создать резервную копию базы данных можно создать командой

```
docker-compose exec web python manage.py dumpdata > fixtures.json 
```

Также можно наполнить базу обращаясь к энподинтам API

### Примеры запросов к ресурсам API YaMDb

-   Получение списка всех комментариев к отзыву  
    _Пример ответа на GET-запрос:_  
    Эндпойнт:  [http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/](http://127.0.0.1:8000/api/v1/titles/%7Btitle_id%7D/reviews/%7Breview_id%7D/comments/)  
    Права доступа: Доступно без токена  
    [  
    {  
    "count": 0,  
    "next": "string",  
    "previous": "string",  
    "results": []  
    }  
    ]
    
-   Добавление комментария к отзыву  
    _Пример POST-запроса:_  
    Эндпойнт:  [http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/](http://127.0.0.1:8000/api/v1/titles/%7Btitle_id%7D/reviews/%7Breview_id%7D/comments/)  
    Права доступа: Аутентифицированные пользователи  
    {  
    "text": "string"  
    }  
    _Пример ответа:_  
    {  
    "id": 0,  
    "text": "string",  
    "author": "string",  
    "pub_date": "2019-08-24T14:15:22Z"  
    }
    
-   Получение комментария к отзыву  
    _Пример ответа на GET-запрос:_  
    Эндпойнт:  [http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/](http://127.0.0.1:8000/api/v1/titles/%7Btitle_id%7D/reviews/%7Breview_id%7D/comments/%7Bcomment_id%7D/)  
    Права доступа: Доступно без токена  
    {  
    "id": 0,  
    "text": "string",  
    "author": "string",  
    "pub_date": "2019-08-24T14:15:22Z"  
    }
    
-   Частичное обновление комментария к отзыву  
    _Пример PATCH-запроса:_  
    Эндпойнт:  [http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/](http://127.0.0.1:8000/api/v1/titles/%7Btitle_id%7D/reviews/%7Breview_id%7D/comments/%7Bcomment_id%7D/)  
    Права доступа: Автор комментария, модератор или администратор  
    {  
    "text": "string"  
    }  
    _Пример ответа:_  
    {  
    "id": 0,  
    "text": "string",  
    "author": "string",  
    "pub_date": "2019-08-24T14:15:22Z"  
    }
    
-   Удаление комментария к отзыву  
    _Пример DELETE-запроса:_  
    Эндпойнт:  [http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/](http://127.0.0.1:8000/api/v1/titles/%7Btitle_id%7D/reviews/%7Breview_id%7D/comments/%7Bcomment_id%7D/)  
    Права доступа: Автор комментария, модератор или администратор