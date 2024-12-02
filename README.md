## Referencia:  https://fullcycle.com.br/docker-e-docker-composer-na-pratica-criando-ambiente-laravel/

# Criar projeto
* composer create-project --prefer-dist laravel/laravel laravel-docker
* php artisan migrate
* php artisan serve

# Dockerfile
* touch  Dockerfile
```sh
FROM wyveo/nginx-php-fpm:latest
```
* touch docker-compose.yaml
```sh
version: '3'
services:
    laravel-app
        build: .
# acessar a porta 8080 de nossa máquina e essa acessará a porta 80 do nosso container
        ports:
            - "8080:80"
        volumes:
            - ./:/usr/share/nginx

# docker-compose up -d
# docker-compose up -d --build
# ln -s public html
```
## Compartilhamento de uma rede interna em docker-compose.yaml
```sh
# Esse serviço deve ser criado em nosso laravel-app e em nosso mysql-app.
networks:
  -app-network
```
## Conectando o Banco de Dados
```sh
# Imagem do mysql
mysql-app:
    image: mysql:8.0
# Portas: tanto a porta de nossa máquina quanto a porta de nosso container serão as mesmas
    ports:
        - "3306:3306"
# variável de ambiente
environment:
  MYSQL_DATABASE: teste
  MYSQL_ROOT_PASSWORD: mazera
```
##  Fora dos serviços a criação da rede propriamente dita
```sh
networks:
  app-network:
    driver: bridge
```
## derrubar o conteiner e subir novamente
* docker-compose down
* docker-compose up -d --build
* docker exec -it laravel-docker_laravel-app_1 bash

## Volume
```sh
# Criaremos uma pasta oculta chamada .docker quando declararmos o nosso volume:
volumes:
  - .docker/dbdata:/var/lib/mysql