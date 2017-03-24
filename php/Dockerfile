FROM php:7.1.3-alpine

MAINTAINER wsmdr wsmdr1986@gmail.com

RUN apk update \
    && apk add --no-cache libmcrypt-dev libpng-dev postgresql-dev cyrus-sasl-dev

RUN docker-php-source extract
RUN cd /
RUN wget http://pecl.php.net/get/xdebug-2.5.1.tgz \
    && tar zxvf xdebug-2.5.1.tgz \
    && mv /xdebug-2.5.1 /usr/src/php/ext/xdebug \
    && wget http://pecl.php.net/get/redis-3.1.1.tgz \
    && tar zxvf redis-3.1.1 \
    && mv /redis-3.1.1 /usr/src/php/ext/redis \
    && wget http://pecl.php.net/get/memcached-3.0.3.tgz \
    && tar zxvf memcached-3.0.3.tgz \
    && mv memcached-3.0.3 usr/src/php/ext/memcached

RUN /usr/local/bin/docker-php-ext-install pdo_mysql sockets gd opcache pcntl zip \
    xdebug redis memcached

EXPOSE 9000
CMD ["php-fpm"]
