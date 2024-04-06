#### 设置 SELinux

- 查看状态
```bash
getenforce
```
- 临时关闭
```bash
setenforce 0
```
- 永久关闭
```bash
sed -e 's|^SELINUX=enforcing|SELINUX=disabled|g' -i.bak /etc/selinux/config
```
#### 设置 Swap分区
- 查看状态
```bash
free -m
```
- 临时关闭
```bash
swapoff -a
```
- 永久关闭
```bash
sed -e '/swap/s/^/#/g' -i /etc/fstab
```
#### 设置主机名
```bash
hostnamectl set-hostname  k8s-master-01
```
#### 设置hosts
```bash
echo "192.168.0.170   k8s-master-01"  >>/etc/hosts
```
#### 关闭防火墙
```bash
systemctl disable --now firewalld
```
