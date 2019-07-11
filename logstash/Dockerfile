ARG ELK_VERSION
FROM docker.elastic.co/logstash/logstash:${ELK_VERSION}
ENV TZ=America/Recife

RUN rm -f /usr/share/logstash/pipeline/logstash.conf
COPY pipeline /usr/share/logstash/pipeline/
COPY patterns /usr/share/logstash/patterns/
