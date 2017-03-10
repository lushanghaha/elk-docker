FROM ubuntu:16.04
MAINTAINER Shang-Lin Lu <leo_lu@cht.com.tw>

RUN apt-get update && apt-get install -y curl && apt-get clean

RUN curl -Lso - https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-5.2.1-linux-x86_64.tar.gz | \
      tar zxf - -C /tmp && \
    mv /tmp/metricbeat-5.2.1-linux-x86_64 /usr/share/metricbeat

RUN curl -Lso /usr/share/metricbeat/beats-dashboards-5.2.1.zip \
      https://artifacts.elastic.co/downloads/beats/beats-dashboards/beats-dashboards-5.2.1.zip

ENV PATH=/usr/share/metricbeat:$PATH

# Mount metricbeat.yml
COPY config/metricbeat.yml /usr/share/metricbeat/metricbeat.yml

WORKDIR /usr/share/metricbeat

RUN mkdir data logs && \
    find /usr/share/metricbeat -type d -exec chmod 0750 {} \; && \
    find /usr/share/metricbeat -type f -exec chmod 0640 {} \; && \
    chmod 0750 /usr/share/metricbeat/metricbeat scripts/* && \
    chmod 0770 data logs

CMD ["metricbeat", "-e"]

