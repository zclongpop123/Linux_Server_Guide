EPEL
--
> EPEL(Extra Packages for Enterprise Linux)是由Fedora Special Interest Group维护的Enterprise Linux（RHEL、CentOS）中经 常用到的包。
- 安装epel镜像源
```bash
dnf install epel-release
```
- 替换epel镜像源地址
```bash
sed -e 's!^metalink=!#metalink=!g' \
    -e 's!^#baseurl=!baseurl=!g' \
    -e 's!//download\.fedoraproject\.org/pub!//mirrors.tuna.tsinghua.edu.cn!g' \
    -e 's!//download\.example/pub!//mirrors.tuna.tsinghua.edu.cn!g' \
    -e 's!http://mirrors!https://mirrors!g' \
    -i /etc/yum.repos.d/epel*.repo
```
- 生成镜像缓存
```bash
dnf makecache
```
