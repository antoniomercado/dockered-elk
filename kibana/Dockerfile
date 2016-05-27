FROM phusion/baseimage

ENV KIBANA_VERSION 4.5.0
WORKDIR /opt

# Dependencies
RUN apt-get update -qq
RUN sudo apt-get install -y -qq nginx-full wget vim

# Kibana
RUN mkdir -p /opt/kibana
RUN wget "https://download.elastic.co/kibana/kibana/kibana_"$KIBANA_VERSION"_amd64.deb"  && \
 chmod +x "kibana_"$KIBANA_VERSION"_amd64.deb" && \
 dpkg -i "kibana_"$KIBANA_VERSION"_amd64.deb"

# Configs
ADD kibana.yml /opt/kibana/config/kibana.yml
ADD entrypoint.sh /opt/kibana/entrypoint.sh

EXPOSE 443

CMD /opt/kibana/entrypoint.sh
