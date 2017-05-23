FROM wsmdr1986/php:5.6.30-fpm-alpine

COPY ./repositories /etc/apk/repositories

RUN apk update && apk add --no-cache graphviz font-adobe-100dpi

RUN docker-php-source extract

COPY ./xhprof-0.9.4/extension /usr/src/php/ext/xhprof

RUN docker-php-ext-install xhprof

RUN docker-php-source delete && apk del --purge

EXPOSE 9000
CMD ["php-fpm"]
