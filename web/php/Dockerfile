FROM wsmdr1986/php:7.1.3-fpm-alpine

COPY ./ext/xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN composer config -g repo.packagist composer https://packagist.phpcomposer.com

RUN mkdir /app

RUN addgroup -g 1000 -S wsmdr \
	&& adduser -u 1000 -D -S -G wsmdr wsmdr

RUN chown wsmdr:wsmdr /app

USER wsmdr

WORKDIR /app
