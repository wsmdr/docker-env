FROM php:7.2.4-cli-alpine

LABEL maintainer="wsmdr1986@gmail.com"

ENV PHPIZE_DEPS \
  autoconf \
  dpkg-dev dpkg \
  file \
  g++ \
  gcc \
  libc-dev \
  make \
  pkgconf \
  re2c \
  pcre-dev

COPY ./repositories /etc/apk/repositories

RUN apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
    && docker-php-source extract \
    && cd /usr/src \
    && wget https://pecl.php.net/get/swoole-2.1.3.tgz \
    && wget https://pecl.php.net/get/redis-4.0.2.tgz \
    && wget wget https://github.com/redis/hiredis/archive/v0.13.3.tar.gz \
    && tar -xzvf v0.13.3.tar.gz
    && cd hiredis-0.13.3
    && make clean
    && make -j \
    && make install
    && cd /usr/src \
    && tar -xzvf swoole-2.1.3.tgz
    && tar -xzvf redis-4.0.2.tgz \
    && mv /usr/src/swoole-2.1.3 /usr/src/php/ext/swoole \
    && mv /usr/src/redis-4.0.2 /usr/src/php/ext/redis \
    && docker-php-ext-configure swoole --enable-async-redis \
    && docker-php-ext-install bcmath pdo_mysql pcntl redis swoole \
    && runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	  )" \
	  && apk add --no-cache --virtual .php-rundeps $runDeps \
    && docker-php-source delete
    && apk del .build-deps \
    && rm -rf hiredis-0.13.3 \
    && rm -rf /usr/src/swoole-2.1.3.tgz \
    && rm -rf /usr/src/redis-4.0.2.tgz \
    && rm -rf /usr/src/v0.13.3.tar.gz

CMD ["php", "-a"]
