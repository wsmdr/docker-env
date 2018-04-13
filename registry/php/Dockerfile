FROM php:7.2.4-fpm-alpine

LABEL maintainer "wsmdr1986@gmail.com"

ENV PHPIZE_DEPS \
  autoconf \
  dpkg-dev dpkg \
  file \
  g++ \
  gcc \
  libc-dev \
  make \
  pkgconf \
  re2c

COPY ./repositories /etc/apk/repositories

RUN apk update \
    && apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
    libmcrypt-dev libpng-dev

RUN docker-php-source extract

RUN mkdir /src
WORKDIR /src

RUN wget http://pecl.php.net/get/mcrypt-1.0.1.tgz \
    && tar -xzf mcrypt-1.0.1.tgz \
    && wget http://pecl.php.net/get/xdebug-2.6.0.tgz \
    && tar -xzf xdebug-2.6.0.tgz \
    && mv /src/xdebug-2.6.0 /usr/src/php/ext/xdebug \
    && mv /src/mcrypt-1.0.1 /usr/src/php/ext/mcrypt \
    && rm -rf /src

RUN docker-php-ext-install mcrypt pdo_mysql sockets gd opcache pcntl zip xdebug

RUN docker-php-source delete && apk del .build-deps

COPY ./composer.phar /usr/local/bin/composer

RUN chmod +x /usr/local/bin/composer

WORKDIR /var/www/html

EXPOSE 9000
CMD ["php-fpm"]
