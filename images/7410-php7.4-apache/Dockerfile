FROM php:7.4-apache

MAINTAINER Thierry NANCY <rascar.quapac@gmail.com>

ENV PHP_INI_DATE_TIMEZONE 'UTC'
ENV PHP_INI_MEMORY_LIMIT 256M

# Updating container with postgres_client_12 libraries 
RUN apt-get update && apt-get install -y gnupg dirmngr wget
RUN echo "deb http://apt-archive.postgresql.org/pub/repos/apt/ bionic-pgdg main" > /etc/apt/sources.list.d/pgdg.list
RUN wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -

RUN apt-get update -y \
    && apt-get install -y --no-install-recommends \
        libpng-dev \
        libldap2-dev \
        libxml2-dev \
        libzip-dev \
        libpq-dev \
        libonig-dev \
        libgmp-dev \
        postgresql-client-12 \
        unzip \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql \
    && docker-php-ext-install bcmath gmp mbstring gettext zip gd xml \
    && mv ${PHP_INI_DIR}/php.ini-production ${PHP_INI_DIR}/php.ini

ENV NOA_VERSION 7410

# Get NOALYSS
ADD https://github.com/Rascarquapac/docker-noalyss/raw/main/noalyss-${NOA_VERSION}.tar.gz  /tmp/noalyss.tar.gz
RUN tar -xzf /tmp/noalyss.tar.gz -C /tmp/ \
     && cd  /var/www/html \
     && cp -rvp /tmp/noalyss/ /var/www/html/ \
     && chown -R www-data:www-data /var/www \
     && chown www-data:www-data /var/www/html/noalyss/include/constant.php

EXPOSE 80
#VOLUME /var/www/documents

CMD ["apache2-foreground"]
