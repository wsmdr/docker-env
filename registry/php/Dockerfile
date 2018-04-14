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

ADD mcrypt-1.0.1.tgz /usr/src

ADD xdebug-2.6.0.tgz /usr/src

COPY ./repositories /etc/apk/repositories

RUN apk update \
    && apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
    libmcrypt-dev libpng-dev \
    && docker-php-source extract \
    && mv /usr/src/xdebug-2.6.0 /usr/src/php/ext/xdebug \
    && mv /usr/src/mcrypt-1.0.1 /usr/src/php/ext/mcrypt \
    && docker-php-ext-install mcrypt pdo_mysql sockets gd opcache pcntl zip xdebug \
    && runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	  )" \
	  && apk add --no-cache --virtual .php-rundeps $runDeps \
    && docker-php-source delete && apk del .build-deps

COPY ./composer.phar /usr/local/bin/composer

RUN chmod +x /usr/local/bin/composer

WORKDIR /var/www/html

EXPOSE 9000
CMD ["php-fpm"]
