FROM logstash:5.1.2
MAINTAINER Shang-Lin Lu <leo_lu@cht.com.tw>

WORKDIR /usr/share/logstash

# Mount logstash.yml
COPY config/logstash.yml /usr/share/logstash/config/logstash.yml

# Mount logstash.conf
COPY pipeline/default.conf /usr/share/logstash/pipeline/logstash.conf

EXPOSE 5044

CMD ["logstash", "-f", "/usr/share/logstash/pipeline/logstash.conf"]
