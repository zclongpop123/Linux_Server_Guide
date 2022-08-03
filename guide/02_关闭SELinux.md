关闭SELinux
--
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
