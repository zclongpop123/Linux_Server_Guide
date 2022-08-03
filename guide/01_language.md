设置语言
--
- 查看系统可用语言
```bash
localectl list-locales
```
- 安装语言包
```bash
dnf install langpacks-zh_CN
```
- 临时切换语言

英文
```bash
LANG="en_US.utf8"
```
中文
```bash
LANG="zh_CN.utf8"
```
- 永久切换语言

英文
```bash
localectl set-locale LANG="en_US.utf8" 
```
中文
```bash
localectl set-locale LANG="zh_CN.utf8" 
```
