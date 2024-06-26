# Git远程仓库管理
---
## 1. 将本地仓库push到远程
### 1.1 本地建立一个仓库，并进行初始化到commit操作，详细步骤见git_basic.md

### 1.2 本地NEW一个SSH密钥
如果`~/.ssh/`中已有历史生成过的密钥（以前根据以下步骤添加的），直接跳过生成密钥，将其添加到GitHub密钥管理即可。

**设置Git的username和email（未尝试过）**
```shell
git config --global user.name "Sereyna"
git config --global user.email "yhsgusx26@163.com"
```
**生成 SSH Key**
_中间会问问题，默认一路回车就行_
```shell
~$ ssh-keygen -t rsa -C "yhsgusx26@163.com"
# 结果如下
Generating public/private rsa key pair.
Enter file in which to save the key (/home/sereyna/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/sereyna/.ssh/id_rsa
Your public key has been saved in /home/sereyna/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:svfY/TSbFC6KH8FhaDhCjVSfCgi12PADFmbqMctNi7s yhsgusx26@163.com
The key's randomart image is:
+---[RSA 3072]----+
|+*o.o+.          |
|=B +. .o o       |
|oo*.o o = o      |
|o B..o + o .     |
| = o  o S o   .  |
|  .    o   . . . |
| .    . . . . =  |
|  .    . = + + + |
| E      o.= ..+  |
+----[SHA256]-----+
```
密钥会默认保存在`~/.ssh/`中


### 1.3 将SSH密钥添加到GitHub密钥管理中
```shell
ls -al ~/.ssh # 列出拥有的密钥
cat ~/.ssh/id_rsa.pub # 将密钥内容显示出来并复制，一定是ssh-rsa开头的
```
**将密钥内容复制到GitHub账号中**
设置 -> SSH -> new SSH key
Title名称随便写，如：Personal laptop

**检查连接是否有效**
```bash
~$ ssh -T git@github.com

Hi Sereyna! You've successfully authenticated, but GitHub does not provide shell access.
```
这样就算成功

### 1.4 进行远程上传
**首先GitHub建立同名仓库**（Gitee同理）
*注意：远程仓库要新建同名仓库！！如果不小心建了不同名的，删除步骤如下*
1. 在==仓库页面==(不是个人信息页面)上方第二行一横排按钮最后一个，点击 "Settings"（设置）按钮。
2. 在==仓库设置页面==中，向下滚动，直到找到 "Danger Zone"（危险区域）部分。
3. 在 "Danger Zone" 部分中，您将看到一个 "Delete this repository"（删除此仓库）的按钮。点击该按钮。
4. 系统会要求您再次确认删除操作。请仔细阅读警告信息，并确保您确定要删除仓库。
5. 如果您确认要删除仓库，请在确认页面的指定文本框中输入您的仓库名称，然后点击 "I understand the consequences, delete this repository"（我理解后果，删除此仓库）按钮。
6. 完成删除后，系统会将您重定向到您的个人帐户页面或组织页面。

**远程上传步骤**
以`LearnPYSpider`文件夹为例
```shell
git remote add origin git@github.com:Sereyna/LearnPYSpider.git
git branch -M main
git push -u origin main
```
执行时加上 -v 参数，你还可以看到每个别名的实际链接地址。
```shell
sereyna@sereyna-ThinkPad-S5:~/PycharmProjects/LearnPYSpider$ git remote 
origin
sereyna@sereyna-ThinkPad-S5:~/PycharmProjects/LearnPYSpider$ git remote -v
origin	git@github.com:Sereyna/LearnPYSpider.git (fetch)
origin	git@github.com:Sereyna/LearnPYSpider.git (push)
```

**Gitee**

Git 全局设置:
```bash
git config --global user.name "红泥"
git config --global user.email "asuradasugo@163.com"
```
远程上传：
```bash
git commit -m "first commit"
git remote add origin https://gitee.com/red-mud-cqq/cnblogs-scrapy.git
git push -u origin "master"
```

### 错误解决
**在git pull、git push等无法连接远程库**
```bash
~$ git push -u origin main

kex_exchange_identification: Connection closed by remote host
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```
但是未指明是`Connection`具体被什么IP和端口关闭的，可初步排除是防火墙拦截，就检查一下`~/.ssh/config`文件，将里面的东西全部注释掉就行。

## 2. 克隆（clone）远程 Git 仓库到本地
拷贝格式
```bash
git clone [url]
```
URL使用http或者ssh皆可
```bash
$ git clone git@github.com:Sereyna/MD_notes.git

Cloning into 'MD_notes'...
remote: Enumerating objects: 80, done.
remote: Counting objects: 100% (80/80), done.
remote: Compressing objects: 100% (58/58), done.
remote: Total 80 (delta 14), reused 75 (delta 9), pack-reused 0
Receiving objects: 100% (80/80), 268.46 KiB | 95.00 KiB/s, done.
Resolving deltas: 100% (14/14), done.
```

```
git config --global --add safe.directory /media/Code/Program/MD_notes
```

### 也可以clone本地仓库到本地

```bash
cd /path/to/your/repo       # 切换到你的Git仓库目录
git clone --bare my_repo my_repo.git  # 创建裸副本
mv my_repo.git /new/path/       # 移动裸副本文件夹
cd /path/to/your/newrepo #切换到你新的Git仓库目录
git clone my_repo.git       # 从裸副本恢复出完整的仓库
```
---
**【引用】**
Git教程 
https://www.runoob.com/git/git-remote-repo.html