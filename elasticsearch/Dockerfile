FROM docker.elastic.co/elasticsearch/elasticsearch:6.8.3

LABEL maintainer="吴泽丰 <13416335317@163.com>"

RUN set -xe \
	&& /usr/share/elasticsearch/bin/elasticsearch-plugin install analysis-smartcn 

COPY ./zip/elasticsearch-analysis-ik-6.8.3.zip /tmp/elasticsearch-analysis-ik.zip

RUN set -xe \
#	&& unzip -n /tmp/elasticsearch-analysis-ik.zip -d /usr/share/elasticsearch/plugins \
	&& /usr/share/elasticsearch/bin/elasticsearch-plugin install -b file:///tmp/elasticsearch-analysis-ik.zip \
    && rm -rf /tmp/elasticsearch-analysis-ik.zip

EXPOSE 9200