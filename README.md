# Kittygram

### Как работать с репозиторием финального задания
##### 1. Шаг первый: настройка контейнеризации. 
Клонируем репозиторий
```python
git clone git@github.com:Nurbek878/kittygram_final.git
```
Убедимся, что Docker запущен; откройте терминал, перейдите в директорию backend/ проекта Kittygram и выполните сборку образа:
```bash
docker build -t kittygram_backend . 
```
Запустим контейнер
```python
docker run --name kittygram_backend_container --rm -p 9000:9000 taski_backend 
```
Откроем в браузере страницу http://localhost:9000/api/; приложение должно ответить.
Перейдем в директорию  frontend/ и соберите образ:
```python
docker build -t kittygram_frontend . 
```
Запустим контейнер в интерактивном режиме, с ключом -it, и свяжим порт 9000 контейнера с портом 9000 хоста:
```python
docker run --rm -it -p 9000:9000 --name kittygram_frontend_test kittygram_frontend 
```
Откроем браузер и убедимся, что фронтенд Taski доступен по адресу http://127.0.0.1:9000/.
В случае успешного локального запуска можно приступать ко второму шагу.


##### 2. Настройка CI/CD
Запустим docker compose с этой конфигурацией на своём компьютере.
```python
docker compose -f docker-compose.production.yml up 
```
Далее соберем статику и применим миграции:
```python
docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
docker compose -f docker-compose.production.yml exec backend python manage.py migrate
```
Проверяем, что страница http://localhost:9000/api/ заработала.

##### 3. Workflow для обновления проекта на сервере
Чтобы обновить проект на продакшене, нужно:
    - скопировать на сервер файл docker-compose.production.yml;
    - выполнить на боевом сервере команду docker compose pull, чтобы скачать с DockerHub на сервер обновлённые образы для контейнеров;
    - перезапустить контейнеры из обновлённых образов.
##### Стек

Python 3.9\
Django 3.2\
Djangorestframework 3.12\
Gunicorn 20.1.0 \
Nginx
Docker
PostgreSQL


##### Автор

- [@nurbek878](https://github.com/Nurbek878)
