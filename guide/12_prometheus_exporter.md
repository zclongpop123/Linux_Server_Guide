Prometheus Exporter
--
- 安装
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
