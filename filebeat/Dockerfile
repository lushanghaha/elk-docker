FROM ubuntu:16.04
MAINTAINER Shang-Lin Lu <leo_lu@cht.com.tw>

RUN apt-get update && apt-get install -y curl && apt-get clean

RUN curl -Lso - https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-5.2.1-linux-x86_64.tar.gz | \
      tar zxf - -C /tmp && \
    mv /tmp/filebeat-5.2.1-linux-x86_64 /usr/share/filebeat

#RUN curl -Lso /usr/share/filebeat/beats-dashboards-5.2.1.zip \
#      https://artifacts.elastic.co/downloads/beats/beats-dashboards/beats-dashboards-5.2.1.zip

ENV PATH=/usr/share/filebeat:$PATH

# Mount filebeat.yml
COPY config/filebeat.yml /usr/share/filebeat/filebeat.yml

# Wait for it!
COPY scripts/wait-for-it.sh /usr/share/filebeat/scripts/wait-for-it.sh

# Provide a non-root user.
# "adduser: Warning: The home directory `/usr/share/filebeat' does not belong to the user you are currently creating."
# this warning will be solve at chown -R root:filebeat.
#RUN addgroup --gid 1000 filebeat && \
#    adduser --disabled-password \
#            --gecos=filebeat \
#            --uid=1000 \
#            --gid=1000 \
#            --home /usr/share/filebeat \
#            filebeat

WORKDIR /usr/share/filebeat

RUN mkdir data logs && \
    find /usr/share/filebeat -type d -exec chmod 0750 {} \; && \
    find /usr/share/filebeat -type f -exec chmod 0640 {} \; && \
    chmod 0750 /usr/share/filebeat/filebeat scripts/* && \
    chmod 0770 data logs

#RUN mkdir data logs && \
#    chown -R root:filebeat . && \
#    find /usr/share/filebeat -type d -exec chmod 0750 {} \; && \
#    find /usr/share/filebeat -type f -exec chmod 0640 {} \; && \
#    chmod 0750 /usr/share/filebeat/filebeat scripts/* && \
#    chmod 0770 data logs

# 先不切換到 filebeat user，因為這次任務得存取從外部掛載進來的 log 資料，
# 由於掛載是設定在 docker-composer.yml，則無法改變目錄權限，故先用 root 來啟動 Filebeat
#USER filebeat

#CMD ["filebeat", "-e"]
# 等 30 秒，因為 Logstash 啟動較慢，未來希望可以參數化，或是動態等
CMD ["./scripts/wait-for-it.sh", "-t", "30", "logstash:5044", "--", "filebeat", "-e"]
