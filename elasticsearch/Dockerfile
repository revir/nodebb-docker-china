FROM elasticsearch:2.3.5
RUN /usr/share/elasticsearch/bin/plugin install mobz/elasticsearch-head

RUN cd /usr/share/elasticsearch/plugins/ && mkdir -p analysis-ik && cd analysis-ik && \
 wget https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v1.9.5/elasticsearch-analysis-ik-1.9.5.zip && \
 unzip elasticsearch-analysis-ik-1.9.5.zip
