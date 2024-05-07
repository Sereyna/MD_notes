
目录
# Linux大全
---
## 防火墙
在Ubuntu中，您可以使用ufw（Uncomplicated Firewall）来管理防火墙。以下是启用和配置基本防火墙的步骤：

首先，确保ufw已经安装。如果没有安装，可以使用以下命令安装：
```bash
sudo apt-sudo apt-get install ufw
```
启用ufw：
```bash
sudo ufw enable
```
（可选）允许特定端口（例如，如果您运行Web服务器，需要允许端口80和443）：
```bash
sudo ufw allow 80
sudo ufw allow 443
```
（可选）你也可以指定协议：
```bash
sudo ufw allow 22/tcp
```
您可以列出所有规则并检查状态：
```bash
sudo ufw status verbose
```
如果需要，您可以禁用ufw：
```bash
sudo ufw disable
# 或者
sudo systemctl stop ufw.service
sudo systemctl disable ufw.service
```

## 强制关机引起磁盘只读问题
强制关机有概率会让挂载的磁盘变成只读文件系统

**修复步骤如下**
1. 打开ubuntu中自带的“磁盘”程序（你可通过搜索程序“磁盘”来找到，也可通过菜单栏找到，还可以终端运行：`gnome-disks`）
2. 找到并单击选中想要修复的磁盘，然后点击左下方齿轮状的按钮，并单击运行`check filesystem`和`Repair filesystem`命令（一般只需要`Repair filesystem`命令）
3. 出现提示单击“确定”继续，完成修复。

## 屏幕截图
![ubuntu屏幕截图默认快捷键](IMG/Screenshot.png "ubuntu屏幕截图默认快捷键")

主要用`Shift+PrtSc`进行屏幕部分截图

## 关于JetBrains系列软件同时打开多个卡电脑的问题
**尝试修改bin/JetBrains64.vmoption**
```bash
-Xms:256M  # 赋予软件的初始内存
-Xmx:2024M  # 赋予软件的最大内存，最大为2048M，把这个改成最大
```

**尝试将软件缓存转移至硬盘**

打开bin/idea.properties，将下面四行设置添加进去：
以pycharm为例
```bash
# Use ${idea.home.path} macro to specify location relative to IDE installation home.
# Use ${xxx} where xxx is any Java property (including defined in previous lines of this file) to refer to its value.
# Note for Windows users: please make sure you're using forward slashes: C:/dir1/dir2.

#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the settings directory.
#---------------------------------------------------------------------
# idea.config.path=${user.home}/.DataGrip/config
idea.config.path=/media/Code/JetBrains_cache/pycharm_cache/config

#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the caches directory.
#---------------------------------------------------------------------
# idea.system.path=${user.home}/.DataGrip/system
idea.system.path=/media/Code/JetBrains_cache/pycharm_cache/system

#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the user-installed plugins directory.
#---------------------------------------------------------------------
# idea.plugins.path=${idea.config.path}/plugins
idea.plugins.path=/media/Code/JetBrains_cache/pycharm_cache/plugins

#---------------------------------------------------------------------
# Uncomment this option if you want to customize a path to the logs directory.
#---------------------------------------------------------------------
# idea.log.path=${idea.system.path}/log
idea.log.path=/media/Code/JetBrains_cache/pycharm_cache/log
```
备注：有可能会引发激活问题，再次复制一下激活码就行