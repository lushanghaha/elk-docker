FROM ubuntu:16.04
MAINTAINER Shang-Lin Lu <leo_lu@cht.com.tw>

RUN apt-get update && apt-get install -y curl && apt-get clean

RUN curl -Lso - https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-5.2.1-linux-x86_64.tar.gz | \
      tar zxf - -C /tmp && \
    mv /tmp/filebeat-5.2.1-linux-x86_64 /usr/share/filebeat

RUN curl -Lso /usr/share/filebeat/beats-dashboards-5.2.1.zip \
      https://artifacts.elastic.co/downloads/beats/beats-dashboards/beats-dashboards-5.2.1.zip

ENV PATH=/usr/share/filebeat:$PATH

# Mount filebeat.yml
COPY config/filebeat.yml /usr/share/filebeat/filebeat.yml

WORKDIR /usr/share/filebeat

RUN mkdir data logs && \
    find /usr/share/filebeat -type d -exec chmod 0750 {} \; && \
    find /usr/share/filebeat -type f -exec chmod 0640 {} \; && \
    chmod 0750 /usr/share/filebeat/filebeat scripts/* && \
    chmod 0770 data logs

CMD ["filebeat", "-e"]
