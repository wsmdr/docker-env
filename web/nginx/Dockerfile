FROM nginx:alpine

RUN addgroup -g 82 -S www-data \
	&& adduser -u 82 -D -S -G www-data www-data

COPY ./nginx.conf /etc/nginx/nginx.conf

ARG DEPLOY_WEB_CONFIG='';

COPY ./${DEPLOY_WEB_CONFIG}.conf /etc/nginx/conf.d/default.conf
