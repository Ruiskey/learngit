本文内容：Git使用笔记
参考内容：
> https://www.liaoxuefeng.com/wiki/896043488029600

## Content

[Toc]



## Git安装

### Linux

首先，你可以试着输入`git`，看看系统有没有安装Git：

```
$ git
The program 'git' is currently not installed. You can install it by typing:
sudo apt-get install git
```

像上面的命令，有很多Linux会友好地告诉你Git没有安装，还会告诉你如何安装Git。

如果你碰巧用Debian或Ubuntu Linux，通过一条就可以直接完成Git的安装，非常简单。

> sudo apt-get install git

### win

官网下载安装



## git版本控制

### 0 创建版本库

同github中**repository**的概念，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

**这里以ubuntu18.04为例**

所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

```shell
$ mkdir learngit
$ cd learngit
$ pwd
/home/rei/learngit
```

第二步，通过`git init`命令把这个目录变成Git可以管理的仓库

```shell
$ git init
已初始化空的 Git 仓库于 /home/rei/learngit/.git/
```

这样就完成了git仓库的构建

### 1 本地仓库添加文件

```
touch readme.md
git add readme.md
git commit -m "create readme file"
```

显示

```bash
[master （根提交） 7b4b8d4] create readme file
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 readme.md
```

查看当前工作区：`git status`

```
rei@Rei:~/learngit$ git status
位于分支 master
无文件要提交，干净的工作区
```

修改文件：` vim readme.md ` 添加如下内容

```
Git has leetcode
Git has ML_note
```

然后再查看 `git status`:

```
rei@Rei:~/learngit$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     readme.md

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

再次提交至版本库

```
rei@Rei:~/learngit$ git add readme.md 
rei@Rei:~/learngit$ git commit -m "Git has leetcode"
[master a94c1cc] Git has leetcode
 1 file changed, 2 insertions(+)
```

这时候输入`git diff`是没有任何修改的，继续修改添加`readme.me`，修改结束后查看：

修改后内容为：

```
Git has leetcode
Git has ML_note
Git has liitle_site
```

`git diff`

```
rei@Rei:~/learngit$ git diff
diff --git a/readme.md b/readme.md
index 8494b27..2fad99b 100644
--- a/readme.md
+++ b/readme.md
@@ -1,2 +1,3 @@
 Git has leetcode
 Git has ML_note
+Git has liitle_site
```



### 2 前后版本切换

我们继续对`readme.md`增添一些内容，比如在文本末添加：`append sanxian`

```
Git has leetcode
Git has ML_note
Git has liitle_site
append sanxian
```

然后再提交一个版本：

```
rei@Rei:~/learngit$ git add readme.md 
rei@Rei:~/learngit$ git commit -m "add sanxian"
[master 27481a5] add sanxian
 1 file changed, 2 insertions(+)
```

下面我们查看提交的历史：`git log`

```
commit 27481a53841b189c7b1d7531a9469b79e89378c9 (HEAD -> master)
Author: Ruiskey <1830369287@qq.com>
Date:   Sat Dec 26 13:49:37 2020 +0800

    add sanxian

commit a94c1cc00a82c6cb7d68bce8406f8dba28a38178
Author: Ruiskey <1830369287@qq.com>
Date:   Sat Dec 26 13:37:17 2020 +0800

    Git has leetcode

commit 7b4b8d4d2f4f7f140e3c3695c5252bba10404c5a
Author: Ruiskey <1830369287@qq.com>
Date:   Sat Dec 26 13:33:18 2020 +0800

    create readme file
```

这里的HEAD表示指针指向的当前版本

#### 回退前一个版本 `git reset`

```
rei@Rei:~/learngit$ git reset --hard HEAD^
HEAD 现在位于 a94c1cc Git has leetcode
```

**这里HEAD^^ : 退回上上个版本，HEAD～100前100个版本**

再次查看版本历史：

```
rei@Rei:~/learngit$ git log
commit a94c1cc00a82c6cb7d68bce8406f8dba28a38178 (HEAD -> master)
Author: Ruiskey <1830369287@qq.com>
Date:   Sat Dec 26 13:37:17 2020 +0800

    Git has leetcode

commit 7b4b8d4d2f4f7f140e3c3695c5252bba10404c5a
Author: Ruiskey <1830369287@qq.com>
Date:   Sat Dec 26 13:33:18 2020 +0800

    create readme file
    
```

现在本地版本库就回到了上一个版本

那么如果我们后悔了，想重新撤回到前一个版本呢？

`git reset --hard <commit id>` 就可以回到commit id 对应的那次版本了，如果忘记了之前的commit id 可以查看历史记录

`git reflog` :

```
27481a5 (HEAD -> master) HEAD@{0}: reset: moving to 27481a
a94c1cc HEAD@{1}: reset: moving to HEAD^
27481a5 (HEAD -> master) HEAD@{2}: commit: add sanxian
a94c1cc HEAD@{3}: commit: Git has leetcode
7b4b8d4 HEAD@{4}: commit (initial): create readme file
```



### 3 工作区 & 暂存区

![](/home/rei/个人知识体系/专业学习/编程相关/git/images/122601.png)

工作区与版本库之间的关系如上所示，本地的文件夹位置即为`工作区`，右边的版本库可拆分为 `暂存区`与`版本分支`

`git add`对应的就是把文件加入暂存区stage

`git commit`对应的就是把文件加入版本分支，也就是HEAD指向的master分支

#### tips

如果你在`git add`之后，在`git commit`之前又一次对文件进行了修改，那这些修改部分是不会被提交的，除非你再一次`git add`，然后`git commit`.



### 4 撤销修改

如果你在工作区对文件进行了错误的修改， 补救方案：

1.没有`git add`时，用`git checkout -- file`

2.已经`git add`时，先`git reset HEAD <file>`回退到1.，再按1.操作

3.已经`git commit`时，用`git reset`回退版本

4.推送到远程库，GG?





### 5 删除文件

在工作区中删除文件：

> rm readme.md

这时侯通过`git status`，git就会告诉你当前工作区与版本库中对比，那些文件被删除了，下面你可以

> git rm readme.md

来删除版本库中的文件，也可以：

> git checkout -- readme.md

来恢复刚才删除的文件









## git远程仓库管理







## git分支管理



## git标签管理



## github 与 gitee





## 自定义Git



## git GUI软件



## Github配置SSH Key

```
https://github.com/xiangshuo1992/preload.git
git@github.com:xiangshuo1992/preload.git
```

这两个地址展示的是同一个项目，但是这两个地址之间有什么联系呢？
前者是https url 直接有效网址打开，但是用户每次通过git提交的时候都要输入用户名和密码，有没有简单的一点的办法，一次配置，永久使用呢？当然，所以有了第二种地址，也就是SSH URL，那如何配置就是本文要分享的内容。
**GitHub配置SSH Key的目的是为了帮助我们在通过git提交代码是，不需要繁琐的验证过程，简化操作流程。**

### 1. 设置git的user name和email

如果你是第一次使用，或者还没有配置过的话需要操作一下命令，自行替换相应字段。

```
git config --global user.name "Luke.Deng"
git config --global user.email  "xiangshuo1992@gmail.com"
```

说明：git config --list 查看当前Git环境所有配置，还可以配置一些命令别名之类的。

### 2. 检查是否存在SSH Key

```
cd ~/.ssh
ls
或者
ll
//看是否存在 id_rsa 和 id_rsa.pub文件，如果存在，说明已经有SSH Key
```

如果没有SSH Key，则需要先生成一下（即没有id_rsa.pub文件）

```
ssh-keygen -t rsa -C "xiangshuo1992@gmail.com"
```

### 3. 获取SSH Key

```
cat id_rsa.pub
//拷贝秘钥 ssh-rsa开头，这里面的所有内容都是SSH Key
```



### 4. GitHub添加SSH Key

GitHub点击用户头像，选择setting，然后新建一个SSH Key，取个名字，把之前拷贝的秘钥复制进去，添加就好啦。



### 5. 验证和修改

测试是否成功配置SSH Key

```
ssh -T git@github.com
//运行结果出现类似如下
Hi xiangshuo1992! You've successfully authenticated, but GitHub does not provide shell access.
```

到此SSH Key就算配置成功了



## git本地仓库关联远程仓库

最近，想要搭建一个多平台的云笔记，找了半天也没有合适的软件，比如印象笔记什么的界面还是不喜欢，查看了各种信息，准备选用**Typora+github**这样的配置，但是在使用`ssh`设置`rsa`的过程中，总是出现无法验证的问题

这个问题体现在

```shell
REI@DESKTOP-Q67MRPL MINGW64 /e/Markdown笔记/git_updown/MD_note (master)
$ ssh-add -K ~/.ssh/id_rsa_mdnote_github
Could not open a connection to your authentication agent.
```

这里总是无法验证，也就是密码总是加不上去，解决办法很简单

```
ssh-agent bash
ssh-add ~/.ssh/id_rsaXXX
```

这样也就通过ssh-add将私钥交给ssh-agent来管理了，然后不同的私钥建立不同的连接位置



关于ssh的内容，还有很多不理解的可以参考

> - [ssh-agent 与 ssh 的区别](https://yijiebuyi.com/blog/4b5c272e7058cb331098250c8e98eb3e.html)
> - [了解ssh代理：ssh-agent](https://www.zsythink.net/archives/2407)
> - [git 配置多个SSH-Key](https://my.oschina.net/stefanzhlg/blog/529403)
> - [同一个Mac，配置多个SSH Key](http://shinancao.cn/2016/12/18/Programming-Git-1/)
> - [廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600/898732864121440)





## 多个项目多个key

**这里的多个项目建立多个key比较麻烦，如果你已经设立了User Key在ssh连接的时候，则优先使用global key，除非另外设置，可以[参考](http://shinancao.cn/2016/12/18/Programming-Git-1/)	**

对于不同的项目，通过建立ssh-agent 添加不同的rsa

然后添加远程项目

```shell
REI@DESKTOP-Q67MRPL MINGW64 /d/资料存放/安工大-学校事务/毕业论文 (master)
$ ssh-add  ~/.ssh/id_rsa_Master_thesis_github
Identity added: /c/Users/REI/.ssh/id_rsa_Master_thesis_github (18130319287@163.com)

REI@DESKTOP-Q67MRPL MINGW64 /d/资料存放/安工大-学校事务/毕业论文 (master)
$ git remote -v

REI@DESKTOP-Q67MRPL MINGW64 /d/资料存放/安工大-学校事务/毕业论文 (master)
$ ssh -T git@github.com
Warning: Permanently added the RSA host key for IP address '13.229.188.59' to the list of known hosts.
Hi Ruiskey/Master_thesis! You've successfully authenticated, but GitHub does not provide shell access.

REI@DESKTOP-Q67MRPL MINGW64 /d/资料存放/安工大-学校事务/毕业论文 (master)
$ git add hello.txt

REI@DESKTOP-Q67MRPL MINGW64 /d/资料存放/安工大-学校事务/毕业论文 (master)
$ git commit -m "add hello.txt"
[master (root-commit) e08060d] add hello.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 hello.txt

REI@DESKTOP-Q67MRPL MINGW64 /d/资料存放/安工大-学校事务/毕业论文 (master)
$ git push -u origin master
fatal: 'origin' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

REI@DESKTOP-Q67MRPL MINGW64 /d/资料存放/安工大-学校事务/毕业论文 (master)
$ git remote add origin git@github.com:Ruiskey/Master_thesis

REI@DESKTOP-Q67MRPL MINGW64 /d/资料存放/安工大-学校事务/毕业论文 (master)
$ git push -u origin master
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 214 bytes | 214.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote:
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/Ruiskey/Master_thesis/pull/new/master
remote:
To github.com:Ruiskey/Master_thesis
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.

```



