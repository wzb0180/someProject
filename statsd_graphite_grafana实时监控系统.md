input -> statsd -> graphite -> grafana(show)

by docker-compose

config.js

        {
            {
            "graphiteHost":"127.0.0.1",
            "graphitePort":"2023",
            "port":8125,
            "flushInterval":1000,
            "deleteIdleStats":true,
            "graphite":{
                      "legacNamespace":false,
                      "globalPrefix":"",
                      "prefixCounter":"",
                      "prefixTimer":"",
                      "prefixGauge":"",
                      "prefixSet":""
                }
            }
DockerFile

        FROM hopsoft/graphite-statsd
        MAINTAINER wen zhanbo "wzb"
        ENV REFRESHED_AT 2016
        ADD config.js /opt/statsd/config.js
        EXPOSE 80 2023 8125/udp

docker-compose.yml

        version: '2'
        services:
          graphite:
            build: .
            ports:
              - "8080:80"
              - "2023:2023"
              - "8125:8125/udp"
            volumes:
              - /var/lib/graphite/conf:/opt/graphite/conf
              - /var/lib/graphite/storage:/opt/graphite/storage/
          grafana:
            image: grafana/grafana
            ports:
              - "80:8080"
              - "8125:8125/udp"
            volumes:
              - /var/lib/grafana:/var/lib/grafana
            environment:
              - GF_AUTH_ANONYMOUS_ENABLED=true
              
statsd使用https://github.com/etsy/statsd/blob/master/docs/metric_types.md
