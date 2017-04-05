FROM php:5.6.30-fpm-alpine

MAINTAINER wsmdr wsmdr1986@gmail.com

RUN apk update \
    && apk add --no-cache libmcrypt-dev libpng-dev cyrus-sasl-dev libmemcached-dev

RUN docker-php-source extract
RUN mkdir /opt && cd /opt
RUN wget http://pecl.php.net/get/xdebug-2.5.1.tgz \
    && tar zxvf xdebug-2.5.1.tgz -C /opt \
    && mv /opt/xdebug-2.5.1 /usr/src/php/ext/xdebug \
    && wget http://pecl.php.net/get/redis-3.1.1.tgz \
    && tar zxvf redis-3.1.1.tgz -C /opt \
    && mv /opt/redis-3.1.1 /usr/src/php/ext/redis \
    && wget http://pecl.php.net/get/memcached-2.2.0.tgz \
    && tar zxvf memcached-2.2.0.tgz -C /opt \
    && mv /opt/memcached-2.2.0 /usr/src/php/ext/memcached

RUN /usr/local/bin/docker-php-ext-install mysql mcrypt pdo_mysql sockets gd opcache pcntl zip \
    xdebug redis memcached

RUN docker-php-source delete && rm -rf /opt && apk del --purge

COPY ./composer.phar /usr/local/bin/composer

RUN chmod o+x /usr/local/bin/composer

EXPOSE 9000
CMD ["php-fpm"]
