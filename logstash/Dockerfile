FROM phusion/baseimage

ENV DEBIAN_FRONTEND noninteractive
ARG LOGSTASH_VER
ENV LOGSTASH_VER ${LOGSTASH_VER:-'2.3.1'}
ARG _ENV
ENV _ENV ${_ENV:-'production'}

WORKDIR /opt

# Dependencies
RUN add-apt-repository ppa:openjdk-r/ppa && \
 apt-get update -qq && \
 apt-get install -y -qq \
 make \
 openjdk-8-jre-headless \
 wget \
 python \
 git


# Install Logstash
RUN wget --quiet "https://download.elastic.co/logstash/logstash/logstash-$LOGSTASH_VER.tar.gz" -O "/opt/logstash-$LOGSTASH_VER.tar.gz" --no-check-certificate && \
 tar zxf logstash-$LOGSTASH_VER.tar.gz && \
 mv logstash-$LOGSTASH_VER logstash

# Install plugins
RUN /opt/logstash/bin/logstash-plugin install logstash-output-zeromq && \
    /opt/logstash/bin/logstash-plugin install logstash-output-s3 && \
    /opt/logstash/bin/logstash-plugin install logstash-output-elasticsearch && \
    /opt/logstash/bin/logstash-plugin install logstash-input-beats && \
    /opt/logstash/bin/logstash-plugin install logstash-filter-drop


# Config files
ADD logstash.conf /etc/logstash/logstash.conf
ADD ./$_ENV/logstash-forwarder.key /etc/logstash/logstash.key
ADD ./$_ENV/logstash-forwarder.crt /etc/logstash/logstash.crt

# Filebeat port
EXPOSE 8444

CMD /opt/logstash/bin/logstash -f /etc/logstash/logstash.conf -v