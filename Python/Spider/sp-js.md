# 爬虫前端处理（主要是JS逆向）
---
## 1. PyExecJS（在Python中直接运行js代码模块）
### 1.1 环境安装
**安装PyExecJS**

```shell
pip install PyExecJS
```
安装成功后，最好重启下cmd终端和pycharm

**安装node.js**

安装node.js是为了让程序有一个比较良好的JS运行引擎
进入官网：https://nodejs.org/en/
选择LTS版本下载（LTS为长期支持版，版本稳定）

解压node.js安装包
```shell
$ sudo tar -xvlf node-v20.12.2-linux-x64.tar.xz -C ~/Environment/
```
创建软链接，使得任意目录下都可以直接使用node和npm命令
```bash
$ sudo ln -s ~/Environment/node-v20.12.2-linux-x64/bin/node /usr/local/bin/node
$ sudo ln -s ~/Environment/node-v20.12.2-linux-x64/bin/npm /usr/local/bin/npm
```

配置环境变量
在/etc/profile中加入如下两行：
```bash
export NODE_HOME=/home/sereyna/Environment/node-v20.12.2-linux-x64/bin/
export PATH=$PATH:$NODE_HOME:/usr/local/bin/
```

```bash
source /etc/profile
```

检查是否安装成功
```bash
$ node -v
$ npm -v
```

验证PyExecJS的JS引擎是否正确
```python
import execjs
print (execjs.get().name)
```
将会输出结果：
```python
Node.js (V8)
```

如果只是支持PyExecJS，后面基本不需要，如需后续配置直接参考
https://blog.csdn.net/jks212454/article/details/131153053
