ARG PHP_VERSION=7.4
ARG NGINX_VERSION=1.17

FROM php:${PHP_VERSION}-fpm-alpine AS PHPAplineBaseProject
ARG USER_ID

RUN \
  apk add --no-cache --virtual .build-deps $PHPIZE_DEPS &&\
  pecl install xdebug-2.9.8 &&\
  docker-php-ext-enable xdebug &&\
  apk del -f .build-deps &&\
  mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

COPY xdebug.ini $PHP_INI_DIR/conf.d
COPY www.conf $PHP_INI_DIR/../php-fpm.d/www.conf

COPY --from=composer:1.10.22 /usr/bin/composer /usr/bin/composer

RUN deluser --remove-home www-data && \
  adduser -D -u $USER_ID -g www-data www-data && \
  install -d -m 0755 -o www-data -g www-data /home/www-data

USER www-data

EXPOSE 9503