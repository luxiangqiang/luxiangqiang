---
title: 小学生都能学会的 Git 之【基础设置】
categories:
- Git版本控制
tags:
- Git版本控制
copyright: true
---

![](/images/git封面图.png)

小鹿带你入门 Git 版本控制工具。

<!--more-->



## Git 入门配置

### 1、配置 user 信息

```java
$ git config --global user.name 'your_name'

$ git config --global user.email 'your_email@domain.com'

```

**config 的三个作用域：**

```
$ git config --local //只对某个仓库有效
$ git config --global //对当前用户所有仓库有效
$ git config --system //对系统所有的登录用户有效
```

**显示 config 的配置（加 --list）**

```
$ git config --list --local
$ git config --list --global 
$ git config --list --system 
```



### 2、建立 Git 仓库

> **注意：**首先切换到你想建立仓库的本地文件夹。



#### 初始化一个git仓库

```
git init 仓库名  //我这里的仓库名为 git_xaiolu
```

```
cd git_xiaolu //切换到仓库文件夹中
```

里边会有一个叫做 `.git  ` 的隐藏文件。

> 后边会讲到文件夹中每个文件有什么作用。



#### 设置新建仓库配置

**① 先查看仓库的当前配置**

```
 git config --global --list
```

![](/images/1545530621003.png)

> 上边信息是我们之前设置过全局仓库的配置，所以本地所有的仓库配置都是统一 `username` 和 `email` 。



> **问题：**我们想要单独配置该仓库的信息怎么办？

那我们使用  `--local`  专门配置当前仓库的配置信息。

```
$ git config --local user.name 'xiaolu'  //修改 name 为 xiaolu
$ git config --local user.email '2645299496@qq.com' //修改email为另一个邮箱地 2645299496@qq.com
```

**② 我们再使用命令查看一下配置信息是否更改。**

```
 git config --local --list
```

![](/images/1545530562286.png)

已更改！



**③ 添加一个名叫做 `xiaolu` 的文件夹**

```
mkdir xiaolu
```

**④ 我们在文件夹中添加一个叫做 `test.txt` 文件。 **

```
cd xiaolu //切换到新建文件夹中
vi test.txt //新建一个文件

输入 : wq 保存并退出。
```

查看当前目录下有有没有我们刚刚创建的 test.txt 文件。

```
ls
```

![](/images/1545531566632.png)



**⑤ 然后提交到 `git` 仓库中。**

```
git add test.txt
```

> 这个命令的作用就是我们将 test.txt 文件提交到缓存中，还有没有真正的提交到我们创建的仓库中去。

```
git status
```

> 我们可以通过上边的命令查看当前提交缓存的文件是否已经在一个叫做「**暂存区**」的地方。

![](/images/1545531755772.png)

上图说明我们的文件已经在缓存中等待用户下一步的提交了。



> 下面我们进行真正的提交到仓库。

```
$ git commit -m 'add new file'  
```

这句命令具体什么意思呢？

`commit `提交的意思，`-m` 后边要加上在这次提交的备注。

> 如果你不写备注，你的团队是知道你提交了什么东西。一个好的备注是至关重要的。

![](/images/1545532288973.png)

提交成功！



**⑥ 查看我们提交的记录**

```
$ git log  //通过此命令可以查看提交的历史记录（也就是提交日志）
```

![](/images/1545532555054.png)

我们可以在日志汇总看到**提交者、提交日期、提交备注**等信息。



















