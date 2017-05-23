FROM wsmdr1986/xhprof

RUN mkdir /app

RUN chown www-data:www-data /app

COPY ./xhprof.ini /usr/local/etc/php/conf.d/docker-php-ext-xhprof.ini

WORKDIR /app
