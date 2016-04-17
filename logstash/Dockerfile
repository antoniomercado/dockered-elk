FROM phusion/baseimage

ENV DEBIAN_FRONTEND noninteractive
ENV LOGSTASH_VER 2.3.1
WORKDIR /opt

# Dependencies
RUN add-apt-repository ppa:openjdk-r/ppa && \
 apt-add-repository ppa:brightbox/ruby-ng && \
 apt-get update -qq && \
 apt-get install -y -qq \
 make \
 openjdk-8-jre-headless \
 wget \
 python \
 git \
 ruby2.2


# Install Logstash
RUN wget --quiet "https://download.elastic.co/logstash/logstash/logstash-$LOGSTASH_VER.tar.gz" -O "/opt/logstash-$LOGSTASH_VER.tar.gz" --no-check-certificate && \
 tar zxf logstash-$LOGSTASH_VER.tar.gz && \
 mv logstash-$LOGSTASH_VER logstash

# Install plugins
RUN /opt/logstash/bin/logstash-plugin install logstash-output-zeromq
RUN /opt/logstash/bin/logstash-plugin install logstash-output-s3
RUN /opt/logstash/bin/logstash-plugin install logstash-output-elasticsearch

# Config files
ADD logstash.conf /etc/logstash/logstash.conf
ADD logstash-courier.key /etc/logstash/logstash-courier.key
ADD logstash-courier.crt /etc/logstash/logstash-courier.crt

RUN git clone https://github.com/driskell/log-courier && \
    cd log-courier && \
    GEM_FILE=`make gem | grep -e "File:" | awk '{print $2}'` && \
    cd /opt/logstash/ && \
    export GEM_HOME=vendor/bundle/jruby/1.9 && \ 
    vendor/jruby/bin/jruby vendor/jruby/bin/gem install /opt/log-courier/$GEM_FILE && \
    cd /opt/log-courier && \
    mkdir -p /opt/logstash/lib/logstash/inputs && mkdir -p /opt/logstash/lib/logstash/outputs && \
    cp -rvf lib/logstash /opt/logstash/lib && \
    cd /opt/logstash && \
    echo 'gem "logstash-input-courier", :path => "/opt/logstash/lib/logstash/inputs/"' >> Gemfile && \
    echo 'gem "logstash-output-courier", :path => "/opt/logstash/lib/logstash/outputs/"' >> Gemfile && \
    bin/logstash-plugin install logstash-input-courier && \
    bin/logstash-plugin install logstash-output-courier

# log-courier port
EXPOSE 8444

CMD /opt/logstash/bin/logstash -f /etc/logstash/logstash.conf -v
