切换运行环境
--
- 查看当前环境
```bash
systemctl get-default
```
- 命令行模式
```bash
systemctl set-default multi-user.target
```
- GUI模式
```bash
systemctl set-default graphical.target
```
