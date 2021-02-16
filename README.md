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
This image is based on the official PHP repository,tag php7.4-apache (Debian buster-slim). It doesn't contain database. So you need to link it with a Postgres database. The following docker-compose.yml proposes a stand alone application including Postgresql and Adminer containers. 
As such Noalyss is available on port 8080 of "localhost" and Adminer on port 8083. Passwords and ports can be adapted according to your choices.

The installation is straightforward:
1. If not already done (!), install the docker-compose tool on your host (i.e. Docker Desktop for Mac OS X)
2. Copy the following docker-compose.yml in a target folder on your local host
3. Run the command "docker-compose up -d" in a shell inside the target folder 
4. Done !

How to Configure Noalyss
------------------- 
1. Launch Noalyss client at http://localhost:8080/noalyss/html/install.php
2. Follow the installation instructions. 
    Default values for paths should be kept as is. 
    Values for password, database should be adpated to your own docker-compose.yml definitions
    Do not forget that following docker-compose.yml defines "noalyss-db" as the "Postgresql Server Adress"
    The install.php file must be suppressed after ending configuration (see instructions)
    At this step he postgresql database is running 
    Create non admin user according to manual
    Import previous folder's dumps or create a new folder

How to use Noalyss
-------------
[Presentation](https://wiki.noalyss.eu/lib/exe/fetch.php?media=noalyss_presentation.pdf), [wiki](https://wiki.noalyss.eu/doku.php), [manuals](http://manuel-fr.noalyss.eu/), [demo site](http://demo.noalyss.eu/) and [videos](https://wiki.noalyss.eu/doku.php?id=tutoriel_video) are available.

How to configure Adminer
-------------
Adminer is not mandatory but helpful to develop plugins or understand Noalyss tables and requests. 

According to current docker-compose.yml file, Adminer is available at http://locahost:8083. You have to configure the access with the following values:
* Adminer server: noalyss-db:5432
* Adminer user: noalyss_sql
* Adminer database: noalyss_sql

Docker compose template
---------------
The "docker-compose.yml" file is available on the [Github repository](https://github.com/Rascarquapac/docker-noalyss). The file looks like:

    version: '3'
    services:
      #Noalyss
      noalyss:
        container_name: noalyss-web
        image: rascar/docker-noalyss:7410
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

