Docker Compose
--
- 下载
```bash
curl -L "https://github.com/docker/compose/releases/download/2.9.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
- 添加执行权限
```bash
chmod +x /usr/local/bin/docker-compose
```
- 创建软连接
```bash
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
- 参考链接
> https://docs.docker.com/compose/install/compose-plugin/#installing-compose-on-linux-systems
> https://github.com/docker/compose/releases
