

![Git](assets/logo@2x.png)



## 分支管理



![](assets\fenzhi.png)

### 创建与合并分支

在[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

![git-br-initial](assets\0.png)

每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长。

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

![git-br-create](assets\0-1563503992283.png)

不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

![git-br-dev-fd](assets\0-1563504020564.png)

假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：

![git-br-ff-merge](assets\0-1563504043641.png)

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

![git-br-rm](assets\0-1563504069907.png)

下面开始实战。

首先，我们创建`dev`分支，然后切换到`dev`分支：

```js
$ git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```js
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

然后，用`git branch`命令查看当前分支：

```js
$ git branch
* dev
  master
```

然后，我们就可以在`dev`分支上正常提交，比如对`readme.txt`做个修改，加上一行：

```js
creating a new branch is quick.
```

然后提交：

```js
$ git add readme.txt
$ git commit -m "branch test"
[dev ff78c00] branch test
 1 file changed, 1 insertion(+)
```

现在，`dev`分支的工作完成，我们就可以切换回`master`分支：

```js
$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

切换回`master`分支后，再查看一个`readme.txt`文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变：

![git-br-on-master](assets\0-1563504139157.png)

现在，我们把`dev`分支的工作成果合并到`master`分支上：

```js
$ git merge dev
Updating 09dc11f..ff78c00
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

`git merge`命令用于合并指定分支到当前分支。合并后，再查看`readme.txt`的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

当然，也不是每次合并都能`Fast-forward`，我们后面会讲其他方式的合并。

合并完成后，就可以放心地删除`dev`分支了：

```js
$ git branch -d dev
Deleted branch dev (was ff78c00).
```

删除后，查看`branch`，就只剩下`master`分支了：

```js
$ git branch
* master
```

### 解决冲突

合并分支往往不是一帆风顺的。

准备新的`feature1`分支，继续我们的新分支开发：

```js
$ git checkout -b feature1
Switched to a new branch 'feature1'
```

修改`readme.txt`最后一行，改为：

```js
creating a new branch is quick and simple.
```

在`feature1`分支上提交：

```js
$ git add readme.txt
$ git commit -m " and simple"
[feature1 c26dbed]  and simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

切换到`master`分支：

```js
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
```

Git还会自动提示我们当前`master`分支比远程的`master`分支要超前1个提交。

在`master`分支上把`readme.txt`文件的最后一行改为：

```js
creating a new branch is quick & simple.
```

提交：

```js
$ git add readme.txt 
$ git commit -m "& simple"
[master 5357755] & simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

现在，`master`分支和`feature1`分支各自都分别有新的提交，变成了这样：

![git-br-feature1](assets\0-1563504177997.png)

这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，我们试试看：

```js
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```

果然冲突了！Git告诉我们，`readme.txt`文件存在冲突，必须手动解决冲突后再提交。`git status`也可以告诉我们冲突的文件：

```js
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

我们可以直接查看readme.txt的内容：

```js
$ cat readme.txt
git is a distributed version control system.
git is free software distributed under the GPL.
git has a mutable index called stage.
git tracks changes.
<<<<<<< HEAD
creating a new branch is quick & simple.
=======
creating a new branch is quick and simple.
>>>>>>> feature1
```

Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改如下后保存：

```js
creating a new branch is quick and simple.
```

再提交：

```js
$ git add readme.txt
$ git commit -m "conflict fixed"
[master c8a6cd4] conflict fixed
```

现在，`master`分支和`feature1`分支变成了下图所示：

![git-br-conflict-merged](assets\0-1563504210888.png)

用带参数的`git log`也可以看到分支的合并情况：

```js
// 查看分支的合并情况，包括分支合并图、一行显示、提交校验码缩略显示。
$ git log --graph --pretty=oneline --abbrev-commit
*   c8a6cd4 (HEAD -> master) conflict fixed
|\
| * c26dbed (feature1)  and simple
* | 5357755 & simple
|/
* ff78c00 branch test
* 09dc11f (origin/master) remove test.txt
* 51c785f add test.txt
* 8798b94 git tracks changes
* 14a390e understand how stage works
* 8206148 append GPL
* 9e390c2 add distributed
* 2752175 wrote a readme file
```

>  `--graph` 查看分支合并图

>  --abbrev-commit  提交校验码缩略显示

最后，删除`feature1`分支：

```js
$ git branch -d feature1
Deleted branch feature1 (was c26dbed).
```

工作完成。

### 分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

下面我们实战一下`--no-ff`方式的`git merge`：

首先，仍然创建并切换`dev`分支：

```js
$ git checkout -b dev
Switched to a new branch 'dev'
```

修改readme.txt文件，并提交一个新的commit：

```js
$ git add readme.txt
$ git commit -m "add merge"
[dev d695fb3] add merge
 1 file changed, 1 insertion(+)
```

现在，我们切换回`master`：

```js
$ git checkout master
Switched to branch 'master'
```

准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：

```js
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。

合并后，我们用`git log`看看分支历史：

```js
$ git log --graph --pretty=oneline --abbrev-commit
*   ba2c977 (HEAD -> master) merge with no-ff
|\
| * d695fb3 (dev) add merge
|/
...
```

可以看到，不使用`Fast forward`模式，merge后就像这样：

![git-no-ff-mode](assets\0-1563504249358.png)

### 分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

在`dev`分支上开发，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

![1561366229027](assets\1561366229027.png)

### Bug分支

软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在`dev`上进行的工作还没有提交：

```js
$ git status
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   hello.js
```

并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

幸好，Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```js
$ git stash
Saved working directory and index state WIP on dev: d695fb3 add merge
```

现在，用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

```js
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git checkout -b issue-101
Switched to a new branch 'issue-101'
```

现在修复bug，需要把“git is free software ...”改为“git is a free software ...”，然后提交：

```js
$ git add readme.txt

$ git commit -m "fix bug 101"
[issue-101 e4224c0] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：

```js
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

$ git branch -d issue-101
Deleted branch issue-101 (was e4224c0).
```

太棒了，原计划两个小时的bug修复只花了5分钟！现在，是时候接着回到`dev`分支干活了！

```js
$ git checkout dev
Switched to branch 'dev'

$ git status
On branch dev
nothing to commit, working tree clean

```

工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

```js
$ git stash list
stash@{0}: WIP on dev: d695fb3 add merge
```

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：

```js
$ git stash pop
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   hello.js

Dropped refs/stash@{0} (9980ae39952affdb59b93632ebcba653932a8b3c)
```

再用`git stash list`查看，就看不到任何stash内容了：

```js
$ git stash list
```

你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```js
$ git stash apply stash@{0}
```

### Feature分支

软件开发中，总有无穷无尽的新的功能要不断添加进来。

添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

现在，你终于接到了一个新任务：开发代号为007的新功能。

于是准备开发：

```js
$ git checkout -b feature-007
Switched to a new branch 'feature-007'
```

5分钟后，开发完毕：

```js
$ git add world.js

$ git status
On branch feature-007
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   world.js

$ git commit -m "add feature 007"
[feature-007 cc408f9] add feature 007
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 world.js

```

切回`dev`，准备合并：

```js
$ git checkout dev
```

一切顺利的话，feature分支和bug分支是类似的，合并，然后删除。

但是！

领导说新功能不上了。

虽然白干了，但是这个包含机密资料的分支还是必须就地销毁：

```js
$ git branch -d feature-007
error: The branch 'feature-007' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature-007'.
```

销毁失败。Git友情提醒，`feature-vulcan`分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的`-D`参数。。

现在我们强行删除：

```js
$ git branch -D feature-007
Deleted branch feature-007 (was cc408f9).
```

删除成功！

### 多人协作

当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

要查看远程库的信息，用`git remote`：

```js
$ git remote
origin
```

或者，用`git remote -v`显示更详细的信息：

```js
$ git remote -v
origin  https://github.com/magicstf/learngit.git (fetch)
origin  https://github.com/magicstf/learngit.git (push)
```

上面显示了可以抓取和推送的`origin`的地址。如果没有推送权限，就看不到push的地址。

#### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```js
$ git push origin master
```

如果要推送其他分支，比如`dev`，就改成：

```
$ git push origin dev
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！

#### 抓取分支

多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。

现在，模拟一个你的小伙伴，可以在另一台电脑（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆：

```js
$ git clone git@github.com:magicstf/learngit.git
Cloning into 'learngit'...
remote: Enumerating objects: 38, done.
remote: Counting objects: 100% (38/38), done.
remote: Compressing objects: 100% (19/19), done.
remote: Total 38 (delta 14), reused 38 (delta 14), pack-reused 0
Receiving objects: 100% (38/38), done.
Resolving deltas: 100% (14/14), done.
```

当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的`master`分支。不信可以用`git branch`命令看看：

```js
$ git branch
* master
```

现在，你的小伙伴要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，于是他用这个命令创建本地`dev`分支：

> 这里可能有的同学会报错，报错信息如下：
>
> fatal: 'origin/dev' is not a commit and a branch 'dev' cannot be created from it
>
> 这是因为远程仓库上没有dev分支，我们要将自己的dev分支推送到远程，这样，你的小伙伴才可以创建本地分支并与远程的dev进行关联,执行如下命令：
>
> ```js
> $ git push origin div
> ```
>
> 你推送上去以后，你的小伙伴还需要重新拉去一下远程仓库：
>
> ```js
> $ git pull
> ```
>
> 这样操作后，再执行如下命令：

```js
$ git checkout -b dev origin/dev
```

现在，他就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程：

```js
$ git add env.txt

$ git commit -m "add env"
[dev 5cb158a] add env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt

$ git push origin dev
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 12 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 339 bytes | 169.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:magicstf/learngit.git
   c5b7a0f..5cb158a  dev -> dev                             
```

你的小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：

```js
$ cat env.txt
env

$ git add env.txt

$ git commit -m "add new env"
[dev f1ebc2c] add new env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt
                                             
$ git push origin dev
To https://github.com/magicstf/learngit.git
 ! [rejected]        dev -> dev (fetch first)
error: failed to push some refs to 'https://github.com/magicstf/learngit.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.  
```

推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

```js
$ git pull
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/magicstf/learngit
   c5b7a0f..5cb158a  dev        -> origin/dev
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```

`git pull`也失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：

```js
$ git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```

再pull：

```js
$ git pull
CONFLICT (add/add): Merge conflict in env.txt
Auto-merging env.txt
Automatic merge failed; fix conflicts and then commit the result.
```

这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的[解决冲突](http://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)完全一样。解决后，提交，再push：

```js
$ git add env.txt

$ git commit -m "fix env conflict"
[dev 399c89f] fix env conflict

$ git push origin dev
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 12 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 431 bytes | 431.00 KiB/s, done.
Total 4 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/magicstf/learngit.git
   5cb158a..399c89f  dev -> dev
```

因此，多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to=origin/<branch> dev`。

## 标签管理

发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），创建和删除标签都是瞬间完成的。

Git有commit，为什么还要引入tag？

“请把上周一的那个版本打包发布，commit号是6a5819e...”

“一串乱七八糟的数字不好找！”

如果换一个办法：

“请把上周一的那个版本打包发布，版本号是v1.2”

“好的，按照tag v1.2查找commit就行！”

所以，tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

#### 创建标签

在Git中打标签非常简单，首先，切换到需要打标签的分支上：

```js
$ git branch
* dev
  master

$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

然后，敲命令`git tag <name>`就可以打一个新标签：

```js
$ git tag v1.0
```

可以用命令`git tag`查看所有标签：

```js
v$ git tag
v1.0
```

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：

```js
$ git log --pretty=oneline --abbrev-commit
69c2536 (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
e4224c0 fix bug 101
ba2c977 merge with no-ff
d695fb3 add merge
c8a6cd4 conflict fixed
5357755 & simple
c26dbed  and simple
ff78c00 branch test
09dc11f remove test.txt
51c785f add test.txt
8798b94 git tracks changes
14a390e understand how stage works
8206148 append GPL
9e390c2 add distributed
2752175 wrote a readme file
```

比方说要对`add merge`这次提交打标签，它对应的commit id是`d695fb3`，敲入命令：

```js
$ git tag v0.9 d695fb3
```

再用命令`git tag`查看标签：

```js
$ git tag
v0.9
v1.0
```

注意，标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息：

```js
$ git show v0.9
commit d695fb39afbefcc195e6740c18a0b8937b368316 (tag: v0.9)
Author: magicstf <736643403@qq.com>
Date:   Mon Jun 24 15:49:06 2019 +0800

    add merge

diff --git a/readme.txt b/readme.txt
index a7d1cda..d61fde4 100644
--- a/readme.txt
+++ b/readme.txt
@@ -3,3 +3,4 @@ git is free software distributed under the GPL.
 git has a mutable index called stage.
 git tracks changes.
 creating a new branch is quick and simple.
+current is dev
```

可以看到，`v0.9`确实打在`add merge`这次提交上。

还可以创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：

```js
$ git tag -a v0.1 -m "version 0.1 released" 2752175
```

用命令`git show <tagname>`可以看到说明文字：

```js
$ git show v0.1
tag v0.1
Tagger: magicstf <736643403@qq.com>
Date:   Mon Jun 24 23:59:31 2019 +0800

version 0.1 released

commit 275217593432e1b3b6fef0b464f487a67efbfcf1 (tag: v0.1)
Author: magicstf <736643403@qq.com>
Date:   Mon Jun 24 09:19:54 2019 +0800

    wrote a readme file

diff --git a/readme.txt b/readme.txt
new file mode 100644
index 0000000..87218be
--- /dev/null
+++ b/readme.txt
@@ -0,0 +1,3 @@
+git is a version control system.
+git is free software
+
```

#### 操作标签

如果标签打错了，也可以删除：

```js
$ git tag -d v0.1
Deleted tag 'v0.1' (was 4a40270)
```

因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令`git push origin <tagname>`：

```js
$ git push origin v1.0
Total 0 (delta 0), reused 0 (delta 0)
To https://github.com/magicstf/learngit.git
 * [new tag]         v1.0 -> v1.0
```

或者，一次性推送全部尚未推送到远程的本地标签：

```js
$ git push origin --tags
Total 0 (delta 0), reused 0 (delta 0)
To https://github.com/magicstf/learngit.git
 * [new tag]         v0.9 -> v0.9
```

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

```js
$ git tag -d v0.9
Deleted tag 'v0.9' (was d695fb3)
```

然后，从远程删除。删除命令也是push，但是格式如下：

```js
$ git push origin :refs/tags/v0.9
To https://github.com/magicstf/learngit.git
 - [deleted]         v0.9
```

要看看是否真的从远程库删除了标签，可以登陆GitHub查看。

## 忽略特殊文件

如果我们项目中某个文件不想提交到远程仓库，该怎么办？

当然，git已经为大家想好办法了，在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

忽略文件的原则是：

1. 忽略操作系统自动生成的文件；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如 /node_modules、/dist目录；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

做个试验，假设我们现在有个config.js，里面存放的是一些配置敏感信息，不能提交到远程仓库，如何实现？

首先我们新增`.gitignore`文件和config.js,然后我们查看状态如下：

```js
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        .gitignore
        config.js

nothing added to commit but untracked files present (use "git add" to track)
```

提示有俩个未追踪的文件，然后我们在`.gitignore`文件中新增如下代码:

```js
config.js
```

然后在查看状态，状态如下：

```js
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        .gitignore

nothing added to commit but untracked files present (use "git add" to track)
```

看到没？ config.js 没有了，这就是`.gitignore`文件的作用。