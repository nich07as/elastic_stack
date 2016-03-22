FROM java:8-jre

RUN apt-get update
RUN apt-get install -y wget

ENV ES_VERSION 2.2.1

WORKDIR /opt



RUN wget -O- https://download.elasticsearch.org/elasticsearch/release/org/elasticsearch/distribution/tar/elasticsearch/$ES_VERSION/elasticsearch-$ES_VERSION.tar.gz | tar xvfz -
RUN mv /opt/elasticsearch-$ES_VERSION /opt/elasticsearch


RUN /opt/elasticsearch/bin/plugin -Des.plugins.staging=true install license
RUN /opt/elasticsearch/bin/plugin -Des.plugins.staging=true install shield
RUN /opt/elasticsearch/bin/plugin -Des.plugins.staging=true install watcher
RUN /opt/elasticsearch/bin/plugin -Des.plugins.staging=true install marvel-agent

VOLUME /data
VOLUME /config

ENV ES_JAVA_OPTS -Des.path.conf=/config

RUN addgroup elasticsearch && adduser --ingroup elasticsearch --disabled-password --gecos '' elasticsearch && echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers 
#RUN chown -R elasticsearch  /opt/ | chown -R elasticsearch  /data/ | chown -R elasticsearch  /config/
RUN chown -R elasticsearch  /opt/ | chown -R elasticsearch  /config/




WORKDIR /opt/elasticsearch/bin
USER elasticsearch

CMD ["/opt/elasticsearch/bin/elasticsearch"]
