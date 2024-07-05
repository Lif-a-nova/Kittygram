# **Kitti**_gram_

### Описание:
**Kittygram** - веб-сервис для любителей котиков. Вы сможете делиться фотографиями и достижениями своих питомцев, а также посмотреть на других.

### Используемые технологии:
[![image](https://www.vectorlogo.zone/logos/python/python-ar21.svg)](https://www.python.org/doc/)   [![image](https://www.vectorlogo.zone/logos/djangoproject/djangoproject-ar21.svg)](https://docs.djangoproject.com/en/4.2/)
[![image](https://www.vectorlogo.zone/logos/docker/docker-icon.svg)](https://docs.docker.com/docker-hub/quickstart/)[![image](https://www.vectorlogo.zone/logos/postgresql/postgresql-horizontal.svg)](https://www.postgresql.org/docs/)   [![image](https://www.vectorlogo.zone/logos/ubuntu/ubuntu-ar21.svg)](https://help.ubuntu.com/)[![image](https://www.vectorlogo.zone/logos/gunicorn/gunicorn-ar21.svg)](https://docs.gunicorn.org/en/stable/)[![image](https://www.vectorlogo.zone/logos/nginx/nginx-ar21.svg)](https://docs.nginx.com/)

## Запуск проекта на сервере:

1. На GitHub форкнуть и клонировать репозиторий:
    ```
    git@github.com:Lif-a-nova/kittygram.git
    ```
2. Создать и активировать виртуальное окружение:
    ```
    python -m venv venv
    source venv/bin/activate
    ```
3. Создать необходимый файл с секретами .env (в корне проекта).
4. Собрать и сбилдить образы на свой DockerHub (из корня проекта):
     ```
    cd frontend
    docker build -t "username"/kittygram_frontend .
    cd ../backend
    docker build -t "username"/kittygram_backend .
    cd ../nginx
    docker build -t "username"/kittygram_gateway .
    
    docker push "username"/kittygram_frontend
    docker push "username"/kittygram_backend
    docker push "username"/kittygram_gateway
    ```
5. Развернуть на рабочем сервере ОС на ядре Linux. (В проекте использовалась серверная версия ОС [Ubuntu](https://ubuntu.com/download/desktop))
6. На сервере выполнить следующее:
    -- Nginx:
    Установить Nginx на сервер:
     ```
    sudo apt install nginx -y 
    - изменить настройки с учетом своего ip и проксирования:
    sudo nano /etc/nginx/sites-enabled/default
    - проверить файл конфигурации на ошибки:
    sudo nginx -t 	
    sudo systemctl start nginx
    ```
    -- Установить и запустит файрвол:
    ```
    sudo ufw allow 'Nginx Full'
    sudo ufw allow OpenSSH
    sudo ufw enable
    sudo ufw status 
    ```
    -- Docker
    Установить Docker и Docker Compose на сервер:
    ```
    sudo apt update
    sudo apt install curl
    curl -fSL https://get.docker.com -o get-docker.sh
    sudo sh ./get-docker.sh
    sudo apt-get install docker-compose-plugin
    ```
    -- Создать в корне каталога пустую дирректорию проекта:
     ```
    sudo mkdir kittygram
    cd kittygram
    - скопировать любым удобным образом 2 файла проекта: 
    docker-compose.production.yml и .env
    ```
    -- Запустить в режиме «демона» файл docker-compose.production.yml:
    ```
    sudo docker compose -f docker-compose.production.yml up -d
    - подождать
    sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
    sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
    sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/ 
    ```
    -- Перезагрузить конфигурацию Nginx:
    ```
    sudo systemctl reload nginx
    ```
7. Не забыть:
    ```
    Пользоваться с удовольствием!
    ```
