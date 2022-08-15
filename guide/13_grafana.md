Grafana
--
- 安装
```bash
docker run -d \
  -p 3000:3000 \
  --name=grafana \
  -v /opt/grafana-storage:/var/lib/grafana \
  --restart=always \
  --name grafana \
  grafana/grafana
 ```
