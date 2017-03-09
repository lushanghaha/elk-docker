FROM logstash:5.1.2
MAINTAINER Shang-Lin Lu <leo_lu@cht.com.tw>

WORKDIR /usr/share/logstash

# Mount logstash.yml. 這是 Logstash 系統設定檔
COPY config/logstash.yml /etc/logstash/logstash.yml

# Mount logstash.conf. 這是 Logstash 怎麼處理 log 的設定檔
COPY pipeline/logstash.conf /usr/share/logstash/pipeline/logstash.conf

# 安裝 beats plugin
RUN logstash-plugin install logstash-input-beats

# 提供 beats 輸入 data 以及檢查 service_healthy 用
EXPOSE 5044 9600

WORKDIR /usr/share/logstash

# Logstash 吃自己寫的設定檔
CMD ["logstash", "-f", "/usr/share/logstash/pipeline/logstash.conf"]
