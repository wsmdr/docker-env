FROM nginx:alpine

RUN addgroup -g 82 -S www-data \
	&& adduser -u 82 -D -S -G www-data www-data

COPY ./nginx.conf /etc/nginx/nginx.conf


COPY ./laravel.conf /etc/nginx/conf.d/default.conf
COPY ./xhprof.conf /etc/nginx/conf.d/xhprof.conf
