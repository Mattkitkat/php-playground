ARG PHP_VERSION=7.4
ARG NGINX_VERSION=1.17
ARG POSTGRESQL_VERSION=11

FROM php:${PHP_VERSION}-fpm-alpine AS ApiPlatform2Screencasts
ARG USER_ID

RUN \
  apk add --no-cache bash postgresql-dev && \
  docker-php-ext-install pdo pdo_pgsql && \
  apk add --no-cache --virtual .build-deps $PHPIZE_DEPS &&\
  pecl install xdebug-2.9.8 &&\
  docker-php-ext-enable xdebug &&\
  apk del -f .build-deps &&\
  mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

COPY xdebug.ini $PHP_INI_DIR/conf.d
COPY www.conf $PHP_INI_DIR/../php-fpm.d/www.conf

COPY --from=composer:2.6.5 /usr/bin/composer /usr/bin/composer

RUN deluser --remove-home www-data && \
  adduser -D -u $USER_ID -g www-data www-data -H

USER www-data

EXPOSE 9503

FROM nginx:${NGINX_VERSION}-alpine AS ApiPlatform2ScreencastsNginx

COPY ./nginx.conf /etc/nginx/nginx.conf

WORKDIR /var/www/html

FROM postgres:${POSTGRESQL_VERSION}-alpine AS ApiPlatform2ScreencastsPostgresql
