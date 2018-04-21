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

ADD swoole-2.1.3.tgz /usr/src

COPY ./repositories /etc/apk/repositories

RUN apk update \
    && apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
    && docker-php-source extract \
    && mv /usr/src/swoole-2.1.3 /usr/src/php/ext/swoole \
    && docker-php-ext-install pdo_mysql swoole \
    && runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	  )" \
	  && apk add --no-cache --virtual .php-rundeps $runDeps \
    && docker-php-source delete && apk del .build-deps

WORKDIR /var/www/html

EXPOSE 9000
CMD ["php-fpm"]
