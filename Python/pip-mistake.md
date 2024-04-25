# pip 报错解决大全
---
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
