# pip 使用文档
---
## pip 更换镜像源
**临时使用:**

在使用pip的时候加参数-i，如下：
```shell
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple numpy
```
**手动切换镜像源:**

```shell
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

**镜像源**
```
阿里云 http://mirrors.aliyun.com/pypi/simple/
 
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
 
豆瓣 http://pypi.douban.com/simple
 
Python官方 https://pypi.python.org/simple/
 
v2ex http://pypi.v2ex.com/simple/
 
中国科学院 http://pypi.mirrors.opencas.cn/simple/
 
清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/
 
中国科学技术大学 [http://pypi.mirrors.ustc.edu.cn/simple/]
 
华中理工大学：[http://pypi.hustunique.com/]
 
山东理工大学：[http://pypi.sdutlinux.org/]
```

**查看更改后的镜像源**
```
pip config list
```
或者
```
pip3 config list
```
## pip 报错解决大全
### 1. 读取包超时
```bash
pip._vendor.urllib3.exceptions.ReadTimeoutError:
HTTPSConnectionPool(host='files.pythonhosted.org', port=443): Read timed out.
```
表示在使用 pip 安装Python包时，与远程服务器建立的HTTPS连接池在指定的超时时间内没有收到响应。这可能是因为网络问题、服务器响应慢或者超时设置过短导致的。

**解决**
设置环境变量
```bash
export PIP_DEFAULT_TIMEOUT=100
```
修改 pip 配置文件（位于 $HOME/.config/pip/pip.conf）：
```bash
[global]
timeout = 100
```
其中 100 是超时时间（单位是秒），你可以根据需要调整这个值。