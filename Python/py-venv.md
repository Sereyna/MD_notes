# 虚拟环境（virtualenv）
---
## 1. 基础操作

**激活虚拟环境**
```bash
source ./venv/bin/activate
```

**退出虚拟环境**
```bash
deactivate
```

**生成requirements.txt文件**
```bash
pip freeze > requirements.txt
```

**安装依赖：下载requirements.txt中的pip包**

```bash
pip install -r requirements.txt
```