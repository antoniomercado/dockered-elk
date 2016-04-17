# Pull base image.
FROM java:openjdk-8

# Set install version
ENV ES_PKG_NAME elasticsearch-2.3.1

# Change this value to suit your needs
ENV ES_ROLE admin
# Change this value to suit your needs
ENV ES_USER admin
# Change this value to suit your needs
ENV ES_PASSWORD changeme

# Install ElasticSearch
RUN \
 cd /tmp && \
 wget https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/tar/elasticsearch/2.3.1/$ES_PKG_NAME.tar.gz && \
 tar xvzf $ES_PKG_NAME.tar.gz && \
 rm -f $ES_PKG_NAME.tar.gz && \
 mv $ES_PKG_NAME /elasticsearch

# Plugins
RUN /elasticsearch/bin/plugin install elasticsearch/license/latest
RUN /elasticsearch/bin/plugin install elasticsearch/shield/latest

# Custom ES config
ADD elasticsearch.yml /elasticsearch/config/elasticsearch.yml
ADD roles.yml /elasticsearch/config/shield/roles.yml

RUN /elasticsearch/bin/shield/esusers useradd $ES_USER -p $ES_PASSWORD -r $ES_ROLE

# Expose ports
EXPOSE 9200
EXPOSE 9300

# Define default command
CMD ["/elasticsearch/bin/elasticsearch", "-Des.insecure.allow.root=true"]
