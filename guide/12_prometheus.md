Prometheus Exporter
--
- 安装 Exporter
```bash
docker run -d -p 9100:9100 \
  -v "/proc:/host/proc:ro" \
  -v "/sys:/host/sys:ro" \
  -v "/:/rootfs:ro" \
  --net="host" \
  --restart=always \
  --name node-exporter \
  prom/node-exporter
 ```
- 创建配置文件
```bash
global:
  scrape_interval:     60s
  evaluation_interval: 60s
 
scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets: ['localhost:9090']
        labels:
          instance: prometheus
 
  - job_name: localhost
    static_configs:
      - targets: ['192.168.10.104:9100']
        labels:
          instance: localhost

  - job_name: server20
    static_configs:
      - targets: ['192.168.10.20:9100']
        labels:
```
- 安装 Promotheus
```bash
docker run  -d \
  -p 9090:9090 \
  -v /opt/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml  \
  --restart=always \
  --name prometheus \
  prom/prometheus
```
