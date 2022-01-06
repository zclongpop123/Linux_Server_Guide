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
  
  参考链接 https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html
```
# /etc/samba/smb.conf

[global]
     workgroup = CONTOSO
     realm = CONTOSO.COM
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

  参考链接 https://www.samba.org/samba/docs/current/man-html/winbindd.8.html
```
# /etc/nsswitch.conf

passwd:     files winbind
group:      files winbind
```
- Kerberos 配置

  参考链接 https://web.mit.edu/kerberos/krb5-latest/doc/admin/conf_files/krb5_conf.html#krb5-conf-5
```
# /etc/krb5.conf

[libdefaults]
 dns_lookup_realm = false
 ticket_lifetime = 24h
 renew_lifetime = 7d
 forwardable = true
 rdns = false
 default_realm = CONTOSO.COM
 default_ccache_name = KEYRING:persistent:%{uid}

[realms]
CONTOSO.COM = {
 kdc = 192.168.10.254:88
 # AD域控服务器IP
 default_domain = CONTOSO.COM
}

[domain_realm]
.contoso.com = CONTOSO.COM
contoso.com = CONTOSO.COM
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
- 测试链接
```bash
wbinfo -t
wbinfo -u
```
