---
title: 小学生都能学会的 Git 之【Github 提交篇】
categories:
- Git版本控制
tags:
- Git版本控制
copyright: true
---

Git 版本控制完整使用流程！

<!--more-->

[TOC]

### 一、创建版本库

#### 1、初始化仓库

```
git init
```



#### 2、添加文件到版本库的缓存区

```
git add xiaolu.txt
```



#### 3、将缓存区文件提交到本地正式仓库

```
git commit -m "wrote a file"
```



#### 4、当前版本库的状态

> 比如修改了内容。

```
git status
```



#### 5、查看修改的内容

```
git diff 文件名
```



#### 6、查看提交记录

> `--pretty=oneline` 用于格式化。 

```
git log --pretty=oneline
```



### 二、时光穿梭

#### 1、版本回退

> 回到上一个版本 `HEAD^`，上上版本就是 `HEAD^^` ，100 个之前的版本 `HEAD~100` 

```
git reset --hard HEAD^
```



#### 2、向后回滚

> 1）如果窗口不关闭的情况下可以找到 `commit id` 。
>
> 2)  如果窗口关闭了，需要使用 `git reflog` 命令找到 `commit id`

```
git reset --hard 1094a // 提供的提交 ID
```

```
// 查看命令历史记录
git reflog
//878eae2 (HEAD -> master) HEAD@{0}: reset: moving to 878ea
//ad3b64b HEAD@{1}: reset: moving to HEAD^
//878eae2 (HEAD -> master) HEAD@{2}: commit: 第二次提交
//ad3b64b HEAD@{3}: commit (initial): xiaolu.txt
```



#### 3、管理修改

> 查看工作区和版本区最新版本的区别。

```
git diff HEAD -- xiaolu.txt

diff --git a/xiaolu.txt b/xiaolu.txt
index fda2ac1..4832c20 100644
--- a/xiaolu.txt
+++ b/xiaolu.txt
@@ -1,3 +1,4 @@
 1111
 2222
-333			    // 版本区
\ No newline at end of file
+333^M
+444               // 工作区
\ No newline at end of file
```



#### 4、撤销修改

- **丢弃工作区的修改。**
- **丢弃缓存区的添加**

> 让这个文件回到最近一次 `git commit` 或 `git add` 时的状态。
>
> 1）文件修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态； 
>
> 2）文件经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态 ；

```
git checkout -- xiaolu.txt
```

> **丢弃缓存区的添加**，就是撤销 `git add` 命令操作。

```
git reset HEAD xiaolu.txt // HEAD 代表最新版本
```



#### 5、删除文件

> 工作区删除了文件，导致工作区和版本库中的版本不一致所以杰西莱有两种选择。
>
> 1）将版本库中的该文件进行删除，然后提交。
>
> 2）不小心误删了工作区的文件，要回复工作区误删除的文件。

```
// 将版本库中的该文件进行删除
git rm test.md
git commit -m '删除test'
// 恢复误删除的文件
git checkout -- test.txt
```



### 三、远程仓库

> 本地仓库和远程的仓库通信需要 SSH 加密的，需要设计一样的秘钥才可以进行通信。



###### ▉ 创建秘钥

本地仓库设置密钥需要和远程仓库及逆行加密通信。

> 在 window 下创建 SSH Key，一路回车。在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。 

```
ssh-keygen -t rsa -C "youremail@example.com"
```



###### ▉ Github设置 key

在远程仓库设置一个或多个公钥知道有哪里的本地仓库要通信。

> 登陆GitHub，打开“Account settings”，“SSH Keys”页面。然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容，点“Add Key”，你就应该看到已经添加的Key 。



#### 1、添加远程仓库

> 1）将本地仓库和远程仓库进行关联之后才可以推送信息。
>
> 2）将本地所有内容推送到远程仓库。

```
// 关联远程仓库
git remote add origin 远程仓库地址

//-u 参数不但会把 master 分支推上去，还会将关联两个分支
git push -u origin master
```



#### 2、克隆仓库

> 从远程克隆下仓库。

```
git clone 仓库地址
```



### 四、创建与合并分支

> HEAD 指针是指向 master 的，master 是指向提交的，通过切换 HEAD 指针来切换指针。



#### 1、创建分支

```
// 创建分支
git branch dev
//切换分支
git checkout dev
// 创建并切换分支
git checkout -b dev
```



#### 2、查看当前分支

```
git branch
* dev
  master
```



#### 3、合并分支

> Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。 

```
// 合并分支
git merge dev
Updating 9220589..b12c1f2
Fast-forward
 xiaolu.txt | 4 +++-
 1 file changed, 3 insertions(+), 1 deletion(-)
```



#### 4、删除分支

```
git branch -d dev
```



### 五、解决冲突

> 创建的新分支与主分支 master 都进行修改了，分支进行了分叉，出现合并冲突，不能正常合并。必须手动解决冲突。

![git-br-feature1](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909115478645b93e2b5ae4dc78da049a0d1704a41000/0) 



```
// 尝试合并,产生冲突
$ git merge feature1
Auto-merging xiaolu.txt
CONFLICT (content): Merge conflict in xiaolu.txt
Automatic merge failed; fix conflicts and then commit the result.
```

```
// 冲突信息
$ git status

On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   xiaolu.txt
```

> 查看文件内容 `vi xiaolu.txt` 可以查看不同分值冲突的内容。 

```
// 查看文件内容
vi xiaolu.txt

1111
2222
333
444
dev·ÖÖ§¸üÐÂ
<<<<<<< HEAD
featrue ·ÖÖ§
master ·ÖÖ§ÐÞ¸Ä
=======
featrue ·ÖÖ§
>>>>>>> feature1
```

![git-br-conflict-merged](https://cdn.liaoxuefeng.com/cdn/files/attachments/00138490913052149c4b2cd9702422aa387ac024943921b000/0) 



> 用 `git log` 带参数的情况看到分治合并情况。

```
// 带参查看合并分支情况
git log --graph --pretty=oneline --abbrev-commit

*   34b90d1 (HEAD -> master) 解决冲突后提交
|\
| * e890412 (feature1) featrue提交
* | 775427c master提交
|/
* b12c1f2 在dev分支提交
* 9220589 删除test文件
* 89bd80c 添加新文件
* 5564077 第三次提交
* 878eae2 第二次提交
* ad3b64b xiaolu.txt

```



### 六、分支管理策略

> 合并分支时，如果可能，Git会用 `Fast forward` 模式，但这种模式下，删除分支后，会丢掉分支信息 。要强制关闭该模式，git 就会在 merge 时生成一个新的 commit。



#### 1、合并分支强制禁用 ff 模式

```

$ git merge --no-ff -m 'merge with no-ff' dev

Merge made by the 'recursive' strategy.
 xiaolu.txt | 1 +
 1 file changed, 1 insertion(+)

```



#### 2、查看合并记录

> 不使用 Fast forward 模式。

```
$ git log --graph --pretty=oneline --abbrev-commit

*   9885e46 (HEAD -> master) merge with no-ff
|\
| * d303c02 (dev) dev分支提交修改
|/
*   34b90d1 解决冲突后提交
|\
| * e890412 featrue提交
* | 775427c master提交
|/
* b12c1f2 在dev分支提交
* 9220589 删除test文件
* 89bd80c 添加新文件
* 5564077 第三次提交
* 878eae2 第二次提交
* ad3b64b xiaolu.txt

```

![git-no-ff-mode](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909222841acf964ec9e6a4629a35a7a30588281bb000/0) 



#### 3、分支策略

![git-br-policy](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0) 

> 实际开发，按照这几个基本原则进行分支管理。
>
> 1）master 主分治，只用于发布新的版本，平时开发不能在此分支。
>
> 2）开发都在 dev 分支开发。每到一个阶段就要合并 master 分支进行版本发布。
>
> 3）多人合作，创建自己的分支，时不时的向 dev 分支合并。



### 七、Bug 分支

> 每当开发中有 Bug 时，需要通过新的临时分支来修复，修复后合并分支，然后临时分支删除。

如果接到修改 Bug ，当前在 dev 分支工作还没提交，又不想提交，一天后才能开发完毕。只能将现场保护起来。

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   test.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   xiaolu.txt

```



#### 1、存储当前开发进度

> 当前的进度存储起来之后，回到主分支，然后创建 `bug` 分支，修改 `bug` 后提交，然后合并到 `master `分支，然后恢复现场进度。

```
// 存储当前开发进度
$ git stash
Saved working directory and index state WIP on master: 9885e46 merge with no-ff
```



#### 2、查看进度存储列表

```
$ git stash list
stash@{0}: WIP on master: 9885e46 merge with no-ff
```



#### 3、恢复开发进度

> 两种方法：
>
> 1）`git stash apply` 恢复后的 stash 并不删除；通过 `git stash drop` 删除。
>
> 2）`git stash pop` 进行删除，stash 存储的开发进度被删除了。



#### 4、恢复指定存储

```
 git stash apply stash@{0}
```



### 八、Feature 分支

> 软件开发出现新功能，需要单独创建 `feature` 分支进行开发。

如果开发增加一个功能，需要在新分支进行开发，但是中途取消了开发，就地需要删除已经开发的。回到主分支，删除该分支。

```
$ git branch -d feature-vulcan
error: The branch 'feature-vulcan' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
```

> 分支还没有进行合并，不能删除分支，所以我们要进行强制删除分支。



#### 1、强制删除分支

```
git branch -D feature-vulcan
```



### 九、多人合作

> 从远程仓库进行克隆，Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin` 



#### 1、远程仓库信息

```
git remove -v 
```



#### 2、推送分支

> 把该分支上的所有**本地提交**推送到**远程库**。推送时，要指定本地分支。Git就会把该分支推送到远程库对应的远程分支上 。

```
git push origin dev
```

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。



#### 3、抓取分支

> 1）多人合作，克隆远程仓库，只能看到本地 `master` 分支。
>
> 2）要在 dev 开发，就必须创建远程 `origin` 的 `dev` 分支到本地，于是他用这个命令创建本地`dev`分支： 

```
//将本地 git 的头指针指向 origin 库的 dev 分支
$ git checkout -b dev origin/dev
```



#### 4、创建远程仓库分支

```
git checkout -b my-test  //在当前分支下创建my-test的本地分支分支
git push origin my-test  //将my-test分支推送到远程
git branch --set-upstream-to=origin/my-test //将本地分支my-test关联到远程分支my-test上   
git branch -a //查看远程分支 
```



#### 5、推送失败

> 1）如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并。
>
> 2）如果合并有冲突，则解决冲突，并在本地提交； 
>
> 3）如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>` 



