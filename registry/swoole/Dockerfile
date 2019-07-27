FROM php:7.2-cli-alpine

LABEL maintainer="wsmdr1986@gmail.com"

ENV PHPIZE_DEPS \
  autoconf \
  dpkg-dev dpkg \
  file \
  g++ \
  gcc \
  libc-dev \
  libpng-dev \
  make \
  pkgconf \
  re2c \
  pcre-dev \
  openssl-dev

# --build-arg timezone=Asia/Shanghai
ARG timezone
# app env: prod pre test dev
ARG app_env=prod
# default use www-data user
ARG work_user=www-data


ENV APP_ENV=${app_env:-"prod"} \
    TIMEZONE=${timezone:-"Asia/Shanghai"} \
    PHPREDIS_VERSION=4.3.0 \
    SWOOLE_VERSION=4.4.1 \
    COMPOSER_VERSION=1.8.6

RUN echo "http://mirrors.aliyun.com/alpine/v3.10/main" > /etc/apk/repositories \
    && echo "http://mirrors.aliyun.com/alpine/v3.10/community" >> /etc/apk/repositories \
    && apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
    && docker-php-source extract \
    && docker-php-ext-install bcmath gd pdo_mysql mbstring sockets zip sysvmsg sysvsem sysvshm \
    && cd /usr/src \
    && curl -O http://pecl.php.net/get/swoole-${SWOOLE_VERSION}.tgz \
    && curl -O http://pecl.php.net/get/redis-${PHPREDIS_VERSION}.tgz \
    && cd /usr/src \
    && tar -xzvf swoole-${SWOOLE_VERSION}.tgz \
    && tar -xzvf redis-${PHPREDIS_VERSION}.tgz \
    && mv /usr/src/swoole-${SWOOLE_VERSION} /usr/src/php/ext/swoole \
    && mv /usr/src/redis-${PHPREDIS_VERSION} /usr/src/php/ext/redis \
    && docker-php-ext-configure swoole --enable-mysqlnd --enable-sockets --enable-openssl --enable-http2 \
    && docker-php-ext-install redis swoole \
    && runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	  )" \
	  && apk add --no-cache --virtual .php-rundeps $runDeps \
    && docker-php-source delete \
    && apk del .build-deps \
    && rm -rf /usr/src/swoole-${SWOOLE_VERSION}.tgz \
    && rm -rf /usr/src/redis-${PHPREDIS_VERSION}.tgz \
    && curl -o /usr/local/bin/composer https://getcomposer.org/download/${COMPOSER_VERSION}/composer.phar \
    && chmod +x /usr/local/bin/composer \
    && echo "[Date]\ndate.timezone=${TIMEZONE}" > /usr/local/etc/php/conf.d/timezone.ini

RUN mkdir -p /app

WORKDIR /app

CMD ["php", "-a"]
