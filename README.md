# Windows AD域用户访问 Linux Samba 服务
- 安装 Samba
```bash
dnf install samba-*
```
- DNS 配置
```
# /etc/resolv.conf

nameserver 192.168.10.254
```
- Samba 配置
```
# /etc/samba/smb.conf

workgroup = TEST
realm = TEST.COM
security = ADS
password server = 192.168.10.254  # password server是AD域控服务器IP

idmap uid = 10000 - 20000
idmap gid = 10000 - 20000
template shell = /sbin/nologin

winbind separator = /
winbind use default domain = yes
winbind enum users = yes
winbind enum groups = yes
encrypt passwords = yes
```
- Winbind 配置
```
# /etc/nsswitch.conf

passwd:     files winbind
group:      files winbind
```
- Kerberos 配置
```
# /etc/krb5.conf

[libdefaults]
 dns_lookup_realm = false
 ticket_lifetime = 24h
 renew_lifetime = 7d
 forwardable = true
 rdns = false
 default_realm = TEST.COM
 default_ccache_name = KEYRING:persistent:%{uid}

[realms]
TEST.COM = {
 kdc = 192.168.10.254:88
 # AD域控服务器IP
 default_domain = TEST.COM
}

[domain_realm]
.test.com = TEST.COM
test.com = TEST.COM
```
- 将服务器加入域
```
net ads join -U administrator
```
- 重启Samba 和 Winbind 服务
```
systemctl restart smb
systemctl restart winbind
```
