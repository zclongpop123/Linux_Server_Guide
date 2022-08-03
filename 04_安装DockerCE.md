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
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
- 把软件仓库地址替换为 TUNA
```bash
sed -i 's+download.docker.com+mirrors.tuna.tsinghua.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo
```
- 安装
```bash
dnf makecache
dnf install docker-ce
```
- 启动
```bash
systemctl enable --now docker
```
- 参考连接
> https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/
