FROM php:7.3-fpm-alpine3.10

LABEL maintainer="wsmdr1986@gmail.com"

ENV PHPIZE_DEPS \
  autoconf \
  dpkg-dev dpkg \
  freetype \
  freetype-dev \
  libjpeg-turbo \
  libjpeg-turbo-dev \
  libpng-dev \
  file \
  libbsd \
  g++ \
  gcc \
  libc-dev \
  make \
  pkgconf \
  re2c \
  pkgconf \
  re2c \
  pcre-dev \
  openssl-dev \
  libzip-dev

ENV APP_ENV=${app_env:-"prod"} \
    TIMEZONE=${timezone:-"Asia/Shanghai"} \
    PHPREDIS_VERSION=4.3.0 \
    MONGODB_VERSION=1.6.0 \
    XDEBUG_VERSION=2.8.0 \
    COMPOSER_VERSION=1.9.1

# --build-arg timezone=Asia/Shanghai
ARG timezone
# app env: prod pre test dev
ARG app_env=prod
# default use www-data user
ARG work_user=www-data

RUN echo "http://mirrors.aliyun.com/alpine/v3.10/main" > /etc/apk/repositories \
    && echo "http://mirrors.aliyun.com/alpine/v3.10/community" >> /etc/apk/repositories \
    && apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
    && docker-php-source extract \
    && docker-php-ext-install bcmath sockets zip sysvmsg sysvsem sysvshm opcache \
    && cd /usr/src \
    && curl -O http://pecl.php.net/get/xdebug-${XDEBUG_VERSION}.tgz \
    && curl -O http://pecl.php.net/get/mongodb-${MONGODB_VERSION}.tgz \
    && curl -O http://pecl.php.net/get/redis-${PHPREDIS_VERSION}.tgz \
    && cd /usr/src \
    && tar -xzvf xdebug-${XDEBUG_VERSION}.tgz \
    && tar -xzvf mongodb-${MONGODB_VERSION}.tgz \
    && tar -xzvf redis-${PHPREDIS_VERSION}.tgz \
    && mv /usr/src/xdebug-${XDEBUG_VERSION} /usr/src/php/ext/xdebug \
    && mv /usr/src/mongodb-${MONGODB_VERSION} /usr/src/php/ext/mongodb \
    && mv /usr/src/redis-${PHPREDIS_VERSION} /usr/src/php/ext/redis \
    && docker-php-ext-configure gd \
        --with-gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ && \
      NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && docker-php-ext-install -j${NPROC} gd redis mongodb xdebug \
    && runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	  )" \
	  && apk add --no-cache --virtual .php-rundeps $runDeps \
    && docker-php-source delete \
    && apk del .build-deps \
    && rm -rf /usr/src/xdebug-${XDEBUG_VERSION}.tgz \
    && rm -rf /usr/src/mongodb-${MONGODB_VERSION}.tgz \
    && rm -rf /usr/src/redis-${PHPREDIS_VERSION}.tgz \
    && curl -o /usr/local/bin/composer https://getcomposer.org/download/${COMPOSER_VERSION}/composer.phar \
    && chmod +x /usr/local/bin/composer \
    && echo "[Date]\ndate.timezone=${TIMEZONE}" > /usr/local/etc/php/conf.d/timezone.ini

RUN mkdir -p /app
WORKDIR /app

EXPOSE 9000
CMD ["php-fpm"]
