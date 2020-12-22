

![Git](assets/logo@2x.png)

## git简介

### 为什么学习git

- **版本管理**
- **多人协作**

### git是什么

> Git是目前世界上最先进的**分布式**版本控制系统（没有之一）。

#### 什么是版本控制系统

如果你用Microsoft Word写过长篇大论，那你一定有这样的经历： 

想删除一个段落，又怕将来想恢复找不回来怎么办？有办法，先把当前文件“另存为……”一个新的Word文件，再接着改，改到一定程度，再“另存为……”一个新文件，这样一直改下去，最后你的Word文档变成了这样：

![lots-of-docs](assets/0.jpeg)

过了一周，你想找回被删除的文字，但是已经记不清删除前保存在哪个文件里了，只好一个一个文件去找，真麻烦。

看着一堆乱七八糟的文件，想保留最新的一个，然后把其他的删掉，又怕哪天会用上，还不敢删，真郁闷。

更要命的是，有些部分需要你的财务同事帮助填写，于是你把文件Copy到U盘里给她（也可能通过Email发送一份给她），然后，你继续修改Word文件。一天后，同事再把Word文件传给你，此时，你必须想想，发给她之后到你收到她的文件期间，你作了哪些改动，得把你的改动和她的部分合并，真困难。

于是你想，如果有一个软件，**不但能自动帮我记录每次文件的改动，还可以让同事协作编辑**，这样就不用自己管理一堆类似的文件了，也不需要把文件传来传去。如果想查看某次改动，只需要在软件里瞄一眼就可以，岂不是很方便？

这个软件用起来就应该像这个样子，能记录每次文件的改动：

| 版本 | 文件名      | 用户 | 说明                   | 日期       |
| :--- | :---------- | :--- | :--------------------- | :--------- |
| 1    | service.doc | 张三 | 删除了软件服务条款5    | 7/12 10:38 |
| 2    | service.doc | 张三 | 增加了License人数限制  | 7/12 18:09 |
| 3    | service.doc | 李四 | 财务部门调整了合同金额 | 7/13 9:51  |
| 4    | service.doc | 张三 | 延长了免费升级周期     | 7/14 15:17 |

#### 集中式vs分布式

> Linus一直痛恨的CVS及SVN都是集中式的版本控制系统，而Git是分布式版本控制系统，集中式和分布式版本控制系统有什么区别呢？

##### 集中式

集中式版本控制系统，版本库是集中存放在中央服务器的。

![central-repo](assets/0-20190526233947050.jpeg)

![éä¸­åççæ¬æ§å¶å¾è§£](assets/centralized.png)

集中式版本管理的代表：

- SVN

##### 分布式

分布式版本控制系统，版本库存在于每一台客户端机器上。



![distributed-repo](assets/0-20190526234018814.jpeg)

![åå¸å¼çæ¬æ§å¶å¾è§£](assets/distributed.png)

分布式版本管理的代表：

- git

## git使用

### 安装 Git

- 下载地址：https://git-scm.com/downloads
- 支持的平台
  - Linux
  - macOS
  - Windows

在命令行中输入以下命令查看 Git 是否安装成功。

```bash
$ git --version

git version 2.21.1 (Apple Git-122.3)
```

> 注：`$` 表示命令提示符，不需要输入它

### Git 的使用方式

使用 Git Bash 命令行

> 在 Windows 上推荐使用安装 Git 自带的 Git Bash，因为 Git Bash 中可以运行一些 Linux 命令，比 Windows 的操作命令更为强大和易用。尤其是当你涉及到一些 Linux 服务器操作时，掌握这些 Linux 命令是有很大的益处的。

打开方式：

- 在开始目录中找到 `Git Bash`
- 在目录（或者目录空白）位置右键，选择 `Git Bash Here`

图形软件

- SourceTree
- TortoiseGit

### git-bash中常用文件操作命令

```bash
# change directory
# cd 目录路径
$ cd 

# print working directory
$ pwd

# list
# ls -a 包括隐藏文件|目录
$ ls

# make directory
# mkdir 目录路径
$ mkdir

# 新建文件
$ touch 文件名

# remove
# rm 文件路径
# “- f ”忽略不存在的文件，强制删除，不给出提示。 
# “- r” 指示rm将参数中列出的全部目录和子目录均递归地删除。 
# rm -rf 目录路径
# 使用这种方式可以删除一些顽固文件|目录
$ rm

# 查看文件，适用于内容比较少的情况
$ cat 文件名

# 查看文件，适用于文件内容比较多的情况
# 退出使用 q
$ less

# 清屏
$ clear

# vi编辑
# i 进入插入模式
# 无论你处于任何模式，esc 都回到命令模式
# :w 保存
# :q 退出
# :wq 保存并退出
# :q! 强制退出不保存
$ vi 文件名
```

> 扩展学习：
>
> - [Linux命令大全----常用文件操作命令](<https://blog.csdn.net/Evankaka/article/details/49227669>)
> - [Linux vi/vim](<https://www.runoob.com/linux/linux-vim.html>)

### git全局配置

> 安装完成后，还需要最后一步设置，在命令行输入：
>
> ```js
> $ git config --global user.name "Your Name"
> $ git config --global user.email "email@example.com"
> ```

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。

> 注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

### 创建版本库

> 什么是版本库呢？版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
>
> 所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

```js
$ mkdir learngit
$ cd learngit
$ pwd
/Users/tiedan/Desktop/learngit
```

> `pwd`命令用于显示当前目录

**注意：为了避免遇到各种莫名其妙的问题，请确保目录名（包括父目录）不包含中文**

> 第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：

```js
$ git init
Initialized empty Git repository in /Users/tiedan/Desktop/learngit/.git/
```

细心的同学可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -a`命令就可以看见。

言归正传，现在我们编写一个`readme.txt`文件，内容如下：

```js
git is a version control system.
git is free software.
```

一定要放到`learngit`目录下（子目录也行），因为这是一个Git仓库，放到其他地方Git再厉害也找不到这个文件。

和把大象放到冰箱需要3步相比，把一个文件放到Git仓库只需要两步。

```js
$ git add readme.txt
// 	执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。
$ git commit -m "one add"
// -m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。
```

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

![](assets\0.jpg)

分支和`HEAD`的概念我们以后再讲。

刚讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

1. 用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；
2. 用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

注意：你可以add多次添加多个文件，或者使用`git add .`将目录内所有修改都添加到暂存区，然后用commit统一提交。

```js
$ git add file1.txt file2.txt
// 或者
$ git add .
// 然后
$ git commit -m "...add"
```

### 状态查看

我们已经成功地添加并提交了一个readme.txt文件，现在，是时候继续工作了，于是，我们继续修改readme.txt文件，改成如下内容：

```js
git is a distributed version control system.
git is free software.
```

现在，运行`git status`命令看看结果：

```js
$ git status
```

`git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，`readme.txt`被修改过了，但还没有准备提交的修改。

虽然Git告诉我们`readme.txt`被修改了，但如果想看具体修改了什么内容，需要用`git diff`这个命令看看：

```js
$ git diff readme.txt
diff --git a/readme.txt b/readme.txt
index ccd90e1..6e31bba 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-git is a version control system.
+git is a distributed version control system.
 git is free software.
\ No newline at end of file
```

`git diff`顾名思义就是查看difference，可以从上面的命令输出看到，我们在第一行添加了一个`distributed`单词。

知道了对`readme.txt`作了什么修改后，再把它提交到仓库就放心多了，提交修改和提交新文件是一样的两步，第一步是`git add`：

```js
$ git add readme.txt
```

同样没有任何输出。在执行第二步`git commit`之前，我们再运行`git status`看看当前仓库的状态：

```js
$ git status
```

`git status`告诉我们，将要被提交的修改包括`readme.txt`，下一步，就可以放心地提交了：

```js
$ git commit -m "add distributed"
```

提交后，我们再用`git status`命令看看仓库的当前状态：

```js
$ git status
On branch master
nothing to commit, working tree clean
```

Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working tree clean）的。

### 版本回退

现在，再练习一次，修改readme.txt文件如下：

```js
git is a distributed version control system.
git is free software distributed under the GPL.
```

然后尝试提交：

```js
$ git add readme.txt
$ git commit -m "add GPL"
```

像这样，你不断对文件进行修改，然后不断提交修改到版本库里, 每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为`commit`。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个`commit`恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

现在，我们回顾一下`readme.txt`文件一共有几个版本被提交到Git仓库里了：

版本1：one add

```js
git is a version control system.
git is free software.
```

版本2：two add

```js
git is a distributed version control system.
git is free software.
```

版本3：three add

```js
git is a distributed version control system.
git is free software distributed under the GPL.
```

在Git中，我们用`git log`命令查看：

```js
$ git log
```

`git log`命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是`two add`，上一次是` add`，最早的一次是`three add`。

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

```js
$ git log --pretty=oneline
```

你看到的一大串类似`8206148...`的是`commit id`（版本号），而且你看到的`commit id`和我的肯定不一样，以你自己的为准。

**每提交一个新版本，实际上Git就会把它们自动串成一条时间线。**

现在我们启动时光穿梭机，准备把`readme.txt`回退到上一个版本，也就是`add distributed`的那个版本，怎么做呢？

首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`8206148...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，往上100个版本，写成`HEAD~100`。

现在，我们要把当前版本`GPL`回退到上一个版本`add distributed`，就可以使用`git reset`命令：

```js
$ git reset --hard HEAD^
HEAD is now at 9e390c2 add distributed
```

> --hard：暂存区和工作区的内容都会被新指向的commit提交内容所替换；

看看`readme.txt`的内容是不是版本`add distributed`：

```js
$ cat readme.txt
git is a distributed version control system.
git is free software
```

还可以继续回退到上一个版本`one add`，不过且慢，然我们用`git log`再看看现在版本库的状态：

```js
$ git log
commit 306dca38b608ee052598c665da4ac257f607ea14 (HEAD -> master)
Author: Tie dan <415045745@qq.com>
Date:   Mon Dec 21 11:33:54 2020 +0800

    add distributed

commit 27e894b84987db334d8e788b5fca6c9d3e0d1220
Author: Tie dan <415045745@qq.com>
Date:   Mon Dec 21 11:30:10 2020 +0800

    one add
```

最新的那个版本`GPL`已经看不到了！好比你从21世纪坐时光穿梭机来到了19世纪，想再回去已经回不去了，肿么办？

办法其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个`GPL`的`commit id`是`1094adb...`，于是就可以指定回到未来的某个版本：

```js
$ git reset --hard 6cce423
HEAD is now at 8206148 two add
```

看看`readme.txt`的内容：

```js
$ cat readme.txt
git is a distributed version control system.
git is free software distributed under the GPL.
```

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把HEAD从指向`GPL`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   └──> ○ GPL
        │
        ○ add distributed
        │
        ○ one add
```

改为指向`add distributed`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   │    ○ GPL
   │    │
   └──> ○ add distributed
        │
        ○ one add
```

然后顺便把工作区的文件更新了。所以你让`HEAD`指向哪个版本号，你就把当前版本定位在哪。

现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的`commit id`怎么办？

在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到` GPL`，就必须找到`GPL`的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令：

```js
$ git reflog
306dca3 (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
6cce423 HEAD@{1}: commit: GPL
306dca3 (HEAD -> master) HEAD@{2}: commit: add distributed
27e894b HEAD@{3}: commit (initial): one add
```

终于舒了口气，从输出可知，`GPL`的commit id是`6cce423`，现在，你又可以乘坐时光机回到未来了。

### 工作区和暂存区

> Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。

#### 工作区（Working Directory）

就是你在电脑里能看到的目录，比如我的`learngit`文件夹就是一个工作区：

<img src="https://tva1.sinaimg.cn/large/0081Kckwly1glvkrso63oj30oo0g6go2.jpg" style="zoom:50%;" />

#### 版本库（Repository）

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

![](assets\0.jpg)

分支和`HEAD`的概念我们以后再讲。

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

### 管理修改

> 为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。

为什么说Git管理的是修改，而不是文件呢？我们还是做实验。第一步，对readme.txt做一个修改，比如加一行内容：

```js
$ cat readme.txt
git is a distributed version control system.
git is free software distributed under the GPL.
git has a mutable index called stage.
git tracks changes.
```

然后，添加：

```js
$ git add readme.txt
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   readme.txt
```

然后，再修改readme.txt：

```js
$ cat readme.txt
git is a distributed version control system.
git is free software distributed under the GPL.
git has a mutable index called stage.
git tracks changes of files.
```

提交：

```js
$ git commit -m "git tracks changes"
[master 8798b94] git tracks changes
 1 file changed, 1 insertion(+)
```

提交后，再看看状态：

```js
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

```

咦，怎么第二次的修改没有被提交？

别激动，我们回顾一下操作过程：

第一次修改 -> `git add` -> 第二次修改 -> `git commit`

你看，我们前面讲了，Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。

提交后，用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别：

```js
$ git diff HEAD -- readme.txt
diff --git a/readme.txt b/readme.txt
index 6c29947..b7b0032 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,4 +1,4 @@
 git is a distributed version control system.
 git is free software distributed under the GPL.
 git has a mutable index called stage.
-git tracks changes.
+git tracks changes of files.

```

>  注意：--与readme.txt中间有一个空格

那怎么提交第二次修改呢 ?

1. 第一次修改 -> `git add` ->`git commit`-> 第二次修改 -> `git add` -> `git commit`

2. 第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`

### 撤销修改 

现在是凌晨两点，你正在赶一份工作报告，你在`readme.txt`中添加了一行：

```js
$ cat readme.txt
git is a distributed version control system.
git is free software distributed under the GPL.
git has a mutable index called stage.
git tracks changes of files.
my stupid boss still prefers SVN.
```

在你准备提交前，一杯咖啡起了作用，你猛然发现了`stupid boss`可能会让你丢掉这个月的奖金！

既然错误发现得很及时，就可以很容易地纠正它。你可以删掉最后一行，手动把文件恢复到上一个版本的状态。如果用`git status`查看一下：

```js
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

你可以发现，Git会告诉你，`git checkout -- file`可以丢弃工作区的修改：

```js
$ git checkout -- readme.txt
```

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

现在，看看`readme.txt`的文件内容：

```js
$ cat readme.txt
git is a distributed version control system.
git is free software distributed under the GPL.
git has a mutable index called stage.
git tracks changes.
```

文件内容果然复原了。

`git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。

现在假定是凌晨3点，你不但写了一些胡话，还`git add`到暂存区了：

```js
$ cat readme.txt
git is a distributed version control system.
git is free software distributed under the GPL.
git has a mutable index called stage.
git tracks changes.
my stupid boss still prefers SVN.

$ git add readme.txt
```

庆幸的是，在`commit`之前，你发现了这个问题。用`git status`查看一下，修改只是添加到了暂存区，还没有提交：

```js
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   readme.txt
```

Git同样告诉我们，用命令`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区：

```js
$ git reset HEAD readme.txt
Unstaged changes after reset:
M       readme.txt
```

`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。

再用`git status`查看一下，现在暂存区是干净的，工作区有修改：

```js
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

还记得如何丢弃工作区的修改吗？

```js
$ git checkout -- readme.txt
$ git status
On branch master
nothing to commit, working tree clean
```

终于可以睡觉了！

### 删除文件

> 在Git中，删除也是一个修改操作，我们实战一下，先添加一个新文件`test.txt`到Git并且提交：

```js
$ git add test.txt
$ git commit -m "add test.txt"
[master 51c785f] add test.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test.txt
```

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了：

```js
$ rm test.txt
```

这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了：

```js
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

```js
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master 09dc11f] remove test.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 test.txt

```

现在，文件就从版本库中被删除了。

> 先手动删除文件，然后使用git rm <file>和git add<file>效果是一样的。

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```js
$ git checkout -- test.txt
```

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

**注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！**

### 远程仓库

- 备份代码

- 多人协作

  Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。怎么分布呢？最早，肯定只有一台机器有一个原始版本库，此后，别的机器可以“克隆”这个原始版本库，而且每台机器的版本库其实都是一样的，并没有主次之分。

  你肯定会想，至少需要两台机器才能玩远程库不是？但是我只有一台电脑，怎么玩？

  其实一台电脑上也是可以克隆多个版本库的，只要不在同一个目录下。不过，现实生活中是不会有人这么傻的在一台电脑上搞几个远程库玩，因为一台电脑上搞几个远程库完全没有意义，而且硬盘挂了会导致所有库都挂掉，所以我也不告诉你在一台电脑上怎么克隆多个仓库。

  实际情况往往是这样，找一台电脑充当服务器的角色，每天24小时开机，其他每个人都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的提交推送到服务器仓库里，也从服务器仓库中拉取别人的提交。

  完全可以自己搭建一台运行Git的服务器，不过现阶段，为了学Git先搭个服务器绝对是小题大作。好在这个世界上有个叫[GitHub](https://github.com/)的神奇的网站，从名字就可以看出，这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。

#### 本地Git仓库和GitHub仓库之间的传输协议

- ##### SSH

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```js
$ ssh-keygen -t rsa -C "youremail@example.com"
```

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

点“Add Key”，你就应该看到已经添加的Key

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。

- ##### HTTPS

  无需设置，直接克隆

> 有了远程仓库，妈妈再也不用担心我的硬盘了。

#### 添加远程库

现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。

首先，登陆GitHub，然后，在右上角找到“+”按钮，点击 "New repository" 创建一个新的仓库：

在Repository name填入`learngit`，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：

![1561347869051](assets\1561347869051.png)

![1561347924469](assets\1561347924469.png)

目前，在GitHub上的这个`learngit`仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

现在，我们根据GitHub的提示，在本地的`learngit`仓库下运行命令：

```js
git remote add origin https://github.com/magicstf/learngit.git
```

请千万注意，把上面的`magicstf`替换成你自己的GitHub账户名，否则，你在本地关联的就是我的远程库，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。

添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

下一步，就可以把本地库的所有内容推送到远程库上：

```js
$ git push -u origin master
Enumerating objects: 20, done.
Counting objects: 100% (20/20), done.
Delta compression using up to 12 threads
Compressing objects: 100% (15/15), done.
Writing objects: 100% (20/20), 1.58 KiB | 323.00 KiB/s, done.
Total 20 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), done.
To https://github.com/magicstf/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.

```

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

![1561348191810](assets\1561348191810.png)

从现在起，只要本地作了提交，就可以通过命令：

```js
$ git push origin master
```

把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

注意：SSH警告

> 当你第一次使用Git的`clone`或者`push`命令连接GitHub时，会得到一个警告：

```js
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```

这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入`yes`回车即可。

Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：

```js
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
```

这个警告只会出现一次，后面的操作就不会有任何警告了。

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

#### 从远程库克隆

我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。

首先，登陆GitHub，创建一个新的仓库，名字叫`gitskills`：

![1561348600923](assets\1561348600923.png)

我们勾选`Initialize this repository with a README`，这样GitHub会自动为我们创建一个`README.md`文件。创建完毕后，可以看到`README.md`文件：

![1561348655294](assets\1561348655294.png)

现在，远程库已经准备好了，下一步是用命令`git clone`克隆一个本地库：

```js
$ git clone git@github.com:magicstf/gitskills.git
Cloning into 'gitskills'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```

注意把Git库的地址换成你自己的，然后进入`gitskills`目录看看，已经有`README.md`文件了：

```
$ cd gitskills
$ ls
README.md
```

如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。
