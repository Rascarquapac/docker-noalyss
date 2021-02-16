# docker-noalyss
Docker image for Noalyss based on PHP docker official images

Supported tags
-------
* 7410-php 7.4-apache

What is Noalyss
-------

Noalyss is an open source accounting  tool based on a Postgresql server and a PHP web client.  Nolayss is multi-users and multi-folders: number of simultaneous users and folders is only limited by server capabilities. It offers classical accounting features plus co-ownership management, analytical accounting and CRM. Plugins for advanced functions are avaible or can be easily developped.

How to run this image 
------------------
This image is based on the officiel PHP repository.

**Important:**  This image don't contains database. So you need to link it with a database container.

Let's use Docker Compose to integrate it with Postgresql and Adminer tool

* Noalyss is available on port 8080 of your local host. You have to install the tool by runnning the installer at

    https://localhost:8080/noalyss/html/install.php
* Adminer is available on port 8083 where you have to configure the access
    https://localhost:8083

**Note:**
* Adminer server: noalyss-db:5432
* Adminer user: noalyss_sql
* Adminer database: noalyss_sql


Create docker-compose.yml file as following:

    version: '3'
    services:
      #Noalyss
      noalyss:
        container_name: noalyss-web
        build: ./docker
        image: docker-noalyss:7410
        ports:
          - "8080:80"
        tty: true
        stdin_open: true
        restart: always
      #PostgreSQL
      noalyss-db:
        container_name: noalyss-db
        image: postgres:12.2
        environment:
          POSTGRES_DB: noalyss_sql
          POSTGRES_USER: noalyss_sql
          POSTGRES_PASSWORD: dany
          PGDATA: /var/lib/postgresql/data
        volumes:
          - ./db-data:/var/lib/postgresql/data
        ports:
          - "5432:5432"
        restart: always
      # Adminer 192.168.1.35:5432
      adminer:
        container_name: noalyss-adminer
        image: adminer
        restart: always
        ports:
          - "8083:8080"

