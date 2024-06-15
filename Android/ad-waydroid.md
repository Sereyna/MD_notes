# 使用waydroid运行安卓游戏
---
Waydroid是安卓模拟器，其实并不准确。我们都知道Windows上有很多安卓模拟器，它们通过虚拟化技术（如VirtualBox）实现在Windows上运行安卓系统。然而Waydroid不太一样，它不使用虚拟化技术，而使用容器技术（非“Virtual”而是“Container”）。

容器是一种操作系统级虚拟化方法，用于在单个控制主机（LXC主机）上运行多个隔离的Linux系统（容器）。它不提供虚拟机，而是提供具有其自己的CPU、存储器、块I/O、网络等空间和资源控制机制的虚拟环境。

（所以Waydroid仅仅支持Linux！！！）

其最浅显的区别就是：虚拟机模拟了硬件环境，容器没有；虚拟机是物理硬件的抽象，容器是应用层的抽象。

具体可以去搜索一下，在这不作过多讨论。 

==waydroid开源链接：https://gitee.com/gfdgd-xi/waydroid-runner 内含详细安装教程==

**下载安装**

从如下链接下载最新版`waydroid-android-image-gapps.deb`
https://sourceforge.net/projects/waydroid-runner-apt-mirror/files/

直接安装即可，但下载速度可能有点慢，可通过gitee下载安装器，运行安装器按指示一步步走：
```bash
git clone https://gitee.com/gfdgd-xi/waydroid-runner
cd waydroid-runner 
make install -j4
```

**安装adb**
```bash
sudo apt install adb
```
下载完成后，可直接在命令行中使用`adb`命令

adb默认安装路径：/usr/lib/android-sdk/platform-tools/adb

**模拟器 adb 路径获取**
直接使用 adb 工具：`$ adb路径 devices`，例如：
```bash
~$ adb devices
或者
~$ /usr/lib/android-sdk/platform-tools/adb devices


```

设置`adb`的`IP 地址`：打开 `设置` - `关于`- `IP地址`，记录第一个`IP` ，将`${记录的IP}:5555`填入`sample.py`的`adb IP`一栏。

