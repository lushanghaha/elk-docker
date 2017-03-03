FROM logstash:5.1.2
MAINTAINER Shang-Lin Lu <leo_lu@cht.com.tw>

WORKDIR /usr/share/logstash

# Mount logstash.yml. 這是 Logstash 系統設定檔
COPY config/logstash.yml /etc/logstash/logstash.yml

# Mount logstash.conf. 這是 Logstash 怎麼處理 log 的設定檔
COPY pipeline/default.conf /usr/share/logstash/pipeline/logstash.conf

# 安裝 beats plugin
RUN logstash-plugin install logstash-input-beats

# 提供 beats 輸入 data
EXPOSE 5044

# Wait for it!
COPY scripts/wait-for-it.sh /usr/share/logstash/scripts/wait-for-it.sh

WORKDIR /usr/share/logstash

# Make that script executable
RUN chmod 0750 scripts/*

# Logstash 吃自己寫的設定檔
CMD ["./scripts/wait-for-it.sh", "elasticsearch:9200", "--", "logstash", "-f", "/usr/share/logstash/pipeline/logstash.conf"]
