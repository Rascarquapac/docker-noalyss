# docker-noalyss
Docker image for Noalyss based on PHP docker official images

Supported tags
-------
* 7410-php 7.4-apache
* 9100-php 7.4-apache

What is Noalyss
-------
Noalyss is an open source accounting  tool based on a Postgresql server and a PHP web client.  Nolayss is multi-users and multi-folders: number of simultaneous users and folders is only limited by server capabilities. It offers classical accounting features plus co-ownership management, analytical accounting and CRM. Plugins for advanced functions are avaible or can be easily developped.

How to run this image 
------------------
This image is based on the official PHP repository,tag php7.4-apache. The docker-compose.yml includes Postgresql, Adminer and Pgadmin4 containers. 
As such Noalyss is available on port 8080 of "localhost", Adminer on port 8083, Pgamin4 on port 5050. Passwords and ports can be adapted according to your choices by editing the docker-compose.yml

The installation is straightforward:
1. Install Docker Desktop (Mac OS X) or any docker-compose tool on your host.
2. Download the docker-compose.yml and the Dockerfile inside a destination folder.
3. cd inside the destination folder
4. Run the command "docker build ." . It creates an docker image with PHP, postgres-client, Noalyss and its plugins
5. Run the command "docker-compose up -d" in a shell inside the destination folder 

How to Configure Noalyss
------------------- 
1. From your preferred browser go to the URL http://localhost:8080/noalyss/html/install.php
2. Follow the installation instructions. 
    Values for password, database could be adapted by changing docker-compose.yml definitions
    Do not forget that following docker-compose.yml defines "postgres" as the "Postgresql Server Adress"
    The install.php file must be suppressed after ending configuration (see instructions)
    At this step the postgresql database is running 
    Create non admin user according to Noalyss manual
    Import previous folder's dumps or create a new folder according to Noalyss manual
    Configure Plugins accoridng to Noalyss manual

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
