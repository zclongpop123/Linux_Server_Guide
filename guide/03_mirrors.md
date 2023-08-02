配置镜像源
--
- 将所有的官方主镜像地址替换为南京大学镜像站地址
```bash
sed -e 's|^mirrorlist=|#mirrorlist=|g' \
    -e 's|^#baseurl=http://dl.rockylinux.org/$contentdir|baseurl=https://mirrors.nju.edu.cn/rocky|g' \
    -i.bak \
    /etc/yum.repos.d/[Rr]ocky*.repo
```
- 更新缓存
```bash
dnf makecache
```

- 参考连接

> https://mirrors.sdu.edu.cn/docs/guide/RockyLinux/
> 
> https://mirrors.nju.edu.cn/help/rocky
> 
> https://developer.aliyun.com/mirror/rockylinux
