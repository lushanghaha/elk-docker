FROM kibana:5.1.2
MAINTAINER Shang-Lin Lu <leo_lu@cht.com.tw>

# Mount kibana.yml
COPY config/kibana.yml /usr/share/kibana/config/kibana.yml

# Wait for it!
COPY scripts/wait-for-it.sh /usr/share/kibana/scripts/wait-for-it.sh

WORKDIR /usr/share/kibana

# Make that script executable
RUN chmod 0750 scripts/*

CMD ["./scripts/wait-for-it.sh", "elasticsearch:9200", "--", "kibana"]
