# PyMySQL
---
>PyMySQL是Python中流行的MySQL数据库驱动程序，它提供了便捷的方法来连接、查询和更新MySQL数据库。

**安装**
```bash
pip install pymysql
```

## 1. 使用说明
**连接数据库**

首先需要建立与MySQL数据库的连接。使用PyMySQL库的示例代码如下：
```py
pymysql.connect(
    host = 'localhost',
    port = 3306,
    user = 'root',
    password = '123456',
    db ='students',
    charset = 'utf8'
)
```

调用`connect` 方法生成一个 `connect` 对象, 通过这个对象来访问数据库

使用 `connect()` 方法与数据库连接成功后，`connect()` 方法返回一个 `connect()` 对象与数据库进行通信时, 向 `connect` 对象发送 `SQL` 查询命令, 并 `connect` 对象接收 `SQL` 查询结果


_connect常用方法_

- `close()`， 关闭数据库连接
- `commit()`，提交当前事务
- `rollback()`，取消当前事务
- `cursor()`，创建一个游标对象用于执行 SQL 查询命令


**cursor 对象**

cursor 对象用于执行 SQL 命令和得到 SQL 查询结果

_常用方法:_

- `close()`，关闭游标对象
- `execute()`，执行一个数据库查询或命令
- `fetchone()`，返回结果集的下一行
- `fetchall()`，返回结果集中所有行

---
# 【引用】
- 软件测试|使用PyMySQL访问MySQL数据库的详细指南 https://zhuanlan.zhihu.com/p/654606408