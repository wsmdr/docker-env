FROM elasticsearch:5.4.0-alpine

COPY ./ik /usr/share/elasticsearch/plugins/ik

EXPOSE 9200 9300
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["elasticsearch"]
