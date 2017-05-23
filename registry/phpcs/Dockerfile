FROM wsmdr1986/php:7.1.3-fpm-alpine

COPY ./phpcs.phar /usr/local/bin/phpcs
COPY ./phpcbf.phar /usr/local/bin/phpcbf

RUN chmod o+x /usr/local/bin/phpcs
RUN chmod o+x /usr/local/bin/phpcbf
