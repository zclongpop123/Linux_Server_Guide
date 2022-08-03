切换运行模式
--
- 查看当前模式
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
