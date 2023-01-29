设置 Swap分区
--
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
