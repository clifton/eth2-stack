FROM prom/prometheus
ADD prometheus.yml /etc/prometheus/
ENTRYPOINT [ "/bin/prometheus" ]
CMD        [ "--web.listen-address=0.0.0.0:9090", \
             "--config.file=/etc/prometheus/prometheus.yml", \
             "--storage.tsdb.path=/prometheus", \
             "--web.console.libraries=/usr/share/prometheus/console_libraries", \
             "--web.console.templates=/usr/share/prometheus/consoles" ]
