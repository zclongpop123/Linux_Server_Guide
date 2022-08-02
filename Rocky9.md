Rocky 9.0 + Windows AD
=
- 安装以下软件包
```bash
dnf install realmd oddjob-mkhomedir oddjob samba-winbind-clients \
      samba-winbind samba-common-tools samba-winbind-krb5-locator
```
- 要在域成员中共享目录或打印机，请安装samba软件包
```bash
dnf install samba
```
- 备份现有的/etc/samba/smb.conf Samba 配置文件
```bash
mv /etc/samba/smb.conf /etc/samba/smb.conf.bak
```
- 加入域。例如，要加入名为ad.example.com的域
```bash
realm join --membership-software=samba --client-software=winbind ad.example.com
```
```bash
systemctl enable --now smb
systemctl enable --now nmb
```
参考连接：

https://access.redhat.com/documentation/zh-cn/red_hat_enterprise_linux/9/html/configuring_and_using_network_file_services/assembly_setting-up-samba-as-an-ad-domain-member-server_assembly_using-samba-as-a-server
