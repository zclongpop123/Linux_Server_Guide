Python PyPI
--
- 临时使用
```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```
> *注意，simple 不能少, 是 https 而不是 http*

- 升级pip
```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U
```
- 配置pip
```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```
- 参考链接
> https://mirrors.tuna.tsinghua.edu.cn/help/pypi/
