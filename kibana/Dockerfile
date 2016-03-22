FROM debian:jessie

# add our user and group first to make sure their IDs get assigned consistently
RUN groupadd -r kibana && useradd -r -g kibana kibana

#RUN apt-get update && apt-get install -y ca-certificates curl --no-install-recommends && rm -rf /var/lib/apt/lists/*
RUN apt-get update
RUN apt-get install -y ca-certificates curl --no-install-recommends
#RUN apt-get install -y apt-utils
#RUN apt-get install -y nano
RUN apt-get install -y supervisor curl
RUN apt-get install -y apache2
RUN a2enmod ssl
RUN service apache2 restart

RUN mkdir -p /opt/kibana/certs

RUN openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout /opt/kibana/certs/kibana.key -out /opt/kibana/certs/kibana.crt -subj "/C=AU/ST=NSW/L=Sydney/O=Elastic/OU=Elastic Consulting"

ENV KIBANA_VERSION 4.4.2
ENV KIBANA_SHA1 6251dbab12722ea1a036d8113963183f077f9fa7

RUN set -x \
	&& curl -fSL "https://download.elastic.co/kibana/kibana/kibana-${KIBANA_VERSION}-linux-x64.tar.gz" -o kibana.tar.gz \
	&& echo "${KIBANA_SHA1}  kibana.tar.gz" | sha1sum -c - \
	&& mkdir -p /opt/kibana \
	&& tar -xz --strip-components=1 -C /opt/kibana -f kibana.tar.gz \
	&& rm kibana.tar.gz

#RUN /opt/kibana/bin/kibana plugin --install elastic/timelion
RUN /opt/kibana/bin/kibana plugin --install elastic/sense
RUN /opt/kibana/bin/kibana plugin --install elasticsearch/marvel/latest
RUN /opt/kibana/bin/kibana plugin --install kibana/timelion
RUN /opt/kibana/bin/kibana plugin --install kibana/shield/latest

ENV PATH /opt/kibana/bin:$PATH
EXPOSE 5601
#USER kibana

CMD ["kibana"]






