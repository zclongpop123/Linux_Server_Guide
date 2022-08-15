Portainer
--
- 安装并启动
```bash
docker run -d -p 9443:9443 \
              --name portainer \
              --restart=always \
              -v /var/run/docker.sock:/var/run/docker.sock \
              -v /home/portainer/data:/data portainer/portainer-ce:latest
```
