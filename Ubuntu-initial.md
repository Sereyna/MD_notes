# ubuntu 24.04LTS 系统初始化配置
---

==不要！不要！不要！在系统的APP商城里下东西！都是阉割过的！==
## 1. 安装软件

### 1.1 安装Java
```bash
# Java
echo "配置java环境"
sudo mkdir /usr/env
sudo tar -xzvf $jdk -C /usr/env/
echo "# java" >> /home/sereyna/.bashrc
echo "export JAVA_HOME=/usr/env/jdk1.8.0_191" >> /home/sereyna/.bashrc
echo "export CLASSPATH=.:$""JAVA_HOME/lib:$""JRE_HOME/lib:$""CLASSPATH" >> /home/sereyna/.bashrc
echo "export PATH=$""JAVA_HOME/bin:$""JRE_HOME/bin:$""PATH" >> /home/sereyna/.bashrc
echo "export JRE_HOME=$""JAVA_HOME/jre" >> /home/sereyna/.bashrc
source /home/sereyna/.bashrc #这句可能不起作用，需要手动执行
echo "java环境配置完成......"
```

### 1.1 安装sogou拼音

去官网下载最新的版本：https://shurufa.sogou.com/

严格参考官网步骤来：https://shurufa.sogou.com/linux/guide

**需要注意！！！！下面这个`Only Show Current Language`默认是勾上的！要把它取消才能搜到`sogoupinyin`**
![IMG](IMG/sogou.png "搜狗")

### 1.2 安装mysql

**安装服务器端**
```bash
sudo apt-get install mysql-server
```

**安装客户端**
```bash
sudo apt-get install mysql-client
```

**安装成功后查看 MySQL 版本**
```bash
mysql -V;    # 注意 V 是大写
```

**修改 mysqld.cnf 配置文件**
```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
在 [mysqld] 最后一行加入： # skip-grant-tables　　　　<-- add # here

修改完配置文件后需要重新启动 mysql ，否则后续无法登录 mysql；
```bash
service mysql restart
```

**查看默认安装的 MySQL 的用户名和密码**
```bash
~$ sudo cat /etc/mysql/debian.cnf

# Automatically generated for Debian scripts. DO NOT TOUCH!
[client]
host     = localhost
user     = debian-sys-maint
password = OT0b2RxAd4HQMgan
socket   = /var/run/mysqld/mysqld.sock
[mysql_upgrade]
host     = localhost
user     = debian-sys-maint
password = OT0b2RxAd4HQMgan
socket   = /var/run/mysqld/mysqld.sock
```

**登录 MySQL**
用上面的`user`和`password`登录 MySQL:
```bash
mysql -u debian-sys-maint -pOT0b2RxAd4HQMgan
```
进入mysql命令行以后按下面一步一步输入就行

```sql
use mysql; # 进入 MySQL
flush privileges; # 刷新权限
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY '123456'; # 修改用户名和密码
flush privileges; # 刷新权限
quit;
```

**重启 MySQL**
```bash
service mysql restart
```
**最后检验一下**
```bash
mysql -u root -p123456
```

### 1.3 安装JetBrains系列产品（破解）
以datagrip为例

**安装datagrip**

shell文件运行就行，安装完成后打开一下，然后退出

**修改datagirp64.vmoption文件**
```bash
sudo vim ~/Environment/DataGrip-2024.1.2/bin/datagrip64.vmoptions
```
在文件最后加上：
```
-javaagent:/home/sereyna/Environment/ja-netfilter/ja-netfilter.jar

--add-opens=java.base/jdk.internal.org.objectweb.asm=ALL-UNNAMED
--add-opens=java.base/jdk.internal.org.objectweb.asm.tree=ALL-UNNAMED
```

**输入破解码**

https://blog.junxu666.top/p/46415.html


---
【引用】

安装MySQL：https://blog.csdn.net/weixin_47156401/article/details/121087961

破解JetBrains：
步骤：https://blog.junxu666.top/p/7624.html
破解码：https://blog.junxu666.top/p/46415.html