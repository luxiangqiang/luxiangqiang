---
title: Github 开源项目工作流程【git】
categories:
- git
tags:
- git
---

![](/images/git封面图.png)

小鹿详细分享 Github 开源项目工作流程。

<!--more-->

### Github 工作流程

### 1、fork 开源项目

> 克隆别人好的开源项目在自己的远程仓库。



###  2、Clone 开源项目

> 将 fork 的项目 clone 到本地仓库，拥有本地的开发环境。



### 3、进行修改项目内容

> **注意：**不建议直接在 master 分支上直接修改。



**① 我们需要另外创建一个分支（并且换分支）进行修改。**

```
$ git checkout -b 分支名字
```

查看当前分支命令：

```
//查看当前本地分支
$ git branch
//查看远程仓库当前分支
$ git branch -a
```

切换分支命令：

```
$ git checkout 分支名
```



**② 更改项目某些信息 **

```
$ git add a.text
```

```
$ git commit -m 'add a.text'
```



**③ 将分支合并到主分支**

```
$ git checkout master //切换到主分支 
```

合并分支到主分支

```
$ git merge 分支名
```



**④ 将本地从仓库修改的项目同步到远程仓库中**

```
$ git push 
```



### 4、 Pull request(提交一个请求)

> 向原作者提交你的项目。



**① New pull request（新建一个请求）**

> github 这时候自动对源仓库和自己的远程仓库进行代码对比，是否存在冲突，如果有冲突就会显示 `Able to merge`(可以合并) 。



**② Create pull request ** 

> 我们就创建一个新的请求。（在请求里边备注向原作者提交的原因或改动内容）



### 5、原作者就会收到一个请求

> Pull request 1.



**① 原作者点进去可以看到别人对自己项目提交的请求。 **



**② merg pull request**

> 如果觉得他人对自己的修改有帮助，就将请求内容合并到自己当前的分支。



### 6、fetch

> 原作者的项目变动，我们仓库的项目怎么进行同步呢？

```
$ git fetch 源项目地址  master:latest (源项目的分支：自己本地项目的分支)
```

注意：本地项目分支也可以是主分支。（不建议直接在主分支修改）



### 7、merge

> 我们将代码 `fetch` 到 `latest` 了，接下来怎么做？

① 切换到主分支进行 `fetch` 的代码合并

```
$ git checkout master //切换到主分支 
$ git merge 分支名
```



### 8、Push

> 本地仓库的代码与原作者的仓库的代码同步了，但是我们的远程仓库还没有同步,我们进行代码同步。

```
$ git push
```













