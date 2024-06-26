Docker CE
--

- 如果你之前安装过 docker，请先删掉
```bash
dnf remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
```
- 安装依赖
```bash
dnf install -y yum-utils device-mapper-persistent-data lvm2
```
- 下载repo文件
```bash
dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
- 把软件仓库地址替换为 NJU
```bash
sed -i 's+https://download.docker.com+https://mirror.nju.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo
```
- 安装
```bash
dnf makecache
dnf install -y docker-ce
```
- 启动
```bash
systemctl enable --now docker
```
```
ln -s /usr/libexec/docker/cli-plugins/docker-compose /usr/local/bin/docker-compose
```
- 参考连接
> https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/


Docker Hub 镜像
--
- 在配置文件 `/etc/docker/daemon.json` 中加入
```bash
cat > /etc/docker/daemon.json << EOF
{
    "registry-mirrors": [
        "https://docker.chenby.cn/"
    ]
}
EOF
```

- 重新启动docker
```bash
systemctl restart docker
```
- 参考连接
> https://mirrors.nju.edu.cn/help/docker-hub
> 
> https://mirrors.ustc.edu.cn/help/dockerhub.html
