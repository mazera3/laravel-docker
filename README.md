# Docker
## Instalação do Docker
* https://docs.docker.com/engine/install/ubuntu/ 
* https://www.hostinger.com.br/tutoriais/instalar-docker-ubuntu

```sh
# Atualize seu Sistema
sudo apt update && sudo apt upgrade

# Instale Pacotes de Pré-requisitos
sudo apt-get install  curl apt-transport-https ca-certificates software-properties-common
# apt-transport-https – permite que o gerenciador de pacotes transfira os tiles e os dados através de https
# ca-certificates – permite que o navegador da web e o sistema verifiquem certificados de segurança
# curl – transfere dados
# software-properties-common – adiciona scripts para gerenciar o software

# Adicione os Repositórios do Docker
# chave GPG
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
# repositório
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" &&  sudo apt update

# repositório do Docker
apt-cache policy docker-ce

# Instalar Docker Ubuntu 
sudo apt install docker-ce

# Verificar Status do Docker
sudo systemctl status docker

# Usar o Docker
sudo docker run hello-world

# procurar por imagens disponíveis
# sudo docker search [search_query]
sudo docker search ubuntu

# download da imagem
# sudo docker pull [image_name]
sudo docker pull

# listar imagens
sudo docker images

# executar a imagem
# sudo docker run -i -t [image]
sudo docker run -i -t ubuntu

# Docker sem privilégios root
sudo usermod -aG docker $(whoami)
# docker [option]
```

## Uso do docker
* https://fullcycle.com.br/docker-e-docker-composer-na-pratica-criando-ambiente-laravel/

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