---
title: Hexo + Next + github 搭建个人博客详细教程
categories:
- 个人博客
tags: 
- 个人博客
- Hexo
---

![](/images/小鹿的博客.png)

每个程序员必不可少的就是博客网站了，一开始我们并不知道可以搭建属于自己的个人博客网站，通常会在CSDN、博客园等别人搭建的博客网站写博客，当你写久了以后会感觉很 low, 一些文章需要审核通过才能发布。索性我们还不如自己搭建一个个人博客，个人博客的设计美化和内容都按照自己喜欢的要求来，然后在阿里云买一个高大上的域名，岂不是很装逼？尽管你没有学过网页 web 开发，但是通过这篇小鹿详细教程可以亲自搭建起属于自己的个人博客。

<!--more-->

# Hexo + Next + github 搭建个人博客详细教程

小鹿的博客网址：[小鹿的博客](luxiangqiang.我爱你)



## 建站前的准备

建站之前我们先要做好准备工作，将相关的工具准备好。

### 安装 Node.js

> **Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效。** 

**安装网址：**https://nodejs.org/zh-cn/ 

验证是否安装成功：打开cmd命令行(win+r 输入cmd回车)执行 ：

```java
node -v
npm -v
```

安装成功之后显示版本号：

![](\images\node-v.png)

![](\images\npm-v.png)

### 安装 Git

> 通常使用 github 的对 git 并不陌生，Git(读音为/gɪt/。)是一个开源的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。 [1]  Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。 

**安装网址：**https://www.git-scm.com/download/win

验证是否安装成功：打开cmd命令行(win+r 输入cmd回车)执行 ：

```java
git --version
```

安装成功之后显示版本号：

![](\images\git-v.png)



## 安装 Hexo

> Hexo 是一个快速、简洁且高效的博客框架。

了解更多关于 Hexo 请查看官方网站：[Hexo](https://hexo.io/zh-cn/)



### 安装 hexo 框架



![](\images\git.png)

```
$ npm install -g hexo-cli
```

这里安装的是hexo最新版本，如果想安装以前的的版本请运行命令`$ npm install -g hexo` 

以上步骤不出问题的话就已经在本地机器上搭建起了 Hexo 环境。

下面介绍 Hexo 的具体使用方法。 



### 初始化Hexo

#### 创建hexo工程 

```
$ hexo init blog
```

创建一个文件夹blog（blog 为文件夹的名字，可改成自己想要的名字），使用 Hexo 命令初始化 blog 为 hexo 工程目录。 

#### 新建POST 

```
$ cd blog
$ hexo new “HelloWorld”
```

进入初始化后的blog文件夹，创建名为HelloWorld的文件，此时会在/blog/sources/_post/目录下生成HelloWorld.md文件。 

#### 生成静态文件

```
$ hexo generate
```

使用 **Hexo** 引擎将 **Markdown** 格式的文件解析成可以使用浏览器查看的 **HTML** 文件，**HTML** 文件存储在**blog/public** 目录下。 

#### **运行hexo服务器** 

```
$ hexo server
```

打开命令行提示的地址，一般是 [http://localhost:4000/]([http://0.0.0.0:4000/)，既可以看到我们的Hexo网站。 

此时 **Helloworld** 文章中没有任何内容。打开 **/blog/sources/_post/** 目录，使用编辑器打开其中的 **HelloWorld.md** 并在其中添加 **markdown** 格式的内容保存，然后重新运行以下命令： 

```
$ hexo generate
$ hexo server
```

**命令的含义**：**hexo generate** 生成静态文件， **hexo server** 启动服务器。默认情况下，访问网址为： `http://localhost:4000/`。 如果重新改变端口号请详细查看官网文档，这里不多介绍。

**注意：**如果在 **HelloWorld.md** 文件中有中文，在网页进行浏览可能出现乱码，解决方式通过编辑器打开**HelloWorld.md** 文件把编码方式改成 **utf-8** 就可以了。



### 安装主题

**Hexo** 提供了默认主题 **landscape**，主题的位置在 **blog** ->**themes** 文件夹下。主题根据自己喜好可以在网上找到，通过 **Git** 进行相应的下载。下面小鹿贴出几个主题可以进行相应的下载，喜欢哪一个可以进行配置到自己的个人博客了。

主题一：[hexo-theme-next](https://github.com/iissnan/hexo-theme-next)（小鹿用的主题是这一个，后期是自己改进美化的）

主题二：[Casper](https://github.com/TryGhost/Casper)

主题三：[daleanthony](https://github.com/daleanthony)

主题四：[hexo-theme-yilia](https://github.com/litten/hexo-theme-yilia)

主题五：[jacman](https://github.com/wuchong/jacman)

主题六：[hexo-theme-apollo](https://github.com/joyceim/hexo-theme-apollo)

小鹿给大家找了一个主题链接可以选择自己喜欢的主题：[更多主题](https://www.zhihu.com/question/24422335)

#### 配置主题

打开 **git-bash** 切换到 **blog -> themes** 目录下，如果在目录 **blog ->themes** 右键选择启动 **git-bash** 就不用切换了。如果在桌面直接启动 **git-bash** ,可通过下边命令切换到 **blog ->themes**目录下。

```
$ cd /blog/themes
```

选好一个主题，复制主题的 github 地址，通过 git 命令进行下载。（例：https://github.com/iissnan/hexo-theme-next 为一个主题的 github 地址）

```
git clone https://github.com/iissnan/hexo-theme-next
```

下载完之后我将文件夹改成 **next** 了（也可以不改，我为了名字简洁点）。

![](/images/next.png)

然后在修改 **/blog/config.yml** 文件，将其中的 **theme** 改成 **next**。（这个是改变主题的地方，如果你用的是其他的主题，将这个 **next** 改成你下载下来的主题的文件夹的名称）

![](/images/修改next.png)

重新运行以下命令，查看更换主题后的效果 ：

```
$ hexo generate
$ hexo server
```



## 申请 Github 免费静态内容空间

### 注册 Github 账号

我们去 [Github](https://github.com/) 的官网进行账号注册 ，注册完成之后我们根据[官方文档](https://git-scm.com/book/zh/v2/GitHub-%E8%B4%A6%E6%88%B7%E7%9A%84%E5%88%9B%E5%BB%BA%E5%92%8C%E9%85%8D%E7%BD%AE)进行配置 ，然后我们使用自己的账户创建一个 **Repository** （仓库）。

点击网站右上角的 **+** 号，选择 **New Repository**( **注意：**创建仓库的名字要你的注册的用户名一致。其他默认，确定创建)。

![](/images/创建仓库.png)

![](/images/创建仓库2.png)

之后你的静态内容空间就已经创建好了，在浏览器输入你的 **your username（用户名）.github.io**  就可以访问了。



## 将 Hexo 上传 Github 上。

步骤一：安装 **deployer-git** （安装部署工具，方便以后更新）

```java
$ npm install hexo-deployer-git --save
```

步骤二：在 **/blog/_config.yml** 中修改 **deploy** 属性(**注意“：”之后有空格 **) 否则配置失败。

```
deploy:
  type: git
  repository: https://github.com/luxiangqiang/luxiangqiang.github.io.git
  branch: master
```

将上方的 **Repository** 换成你申请的 **Git** 仓库地址 

![](/images/github项目地址.png)

使用 **https** 的方式部署每次提交到 **Github** 都要输入用户名和密码，如果嫌麻烦请使用 **SSH** 的方式，百度/谷歌自行搜索。

步骤三：初始化本地仓库。

```
git init
```

步骤四：连接远程仓库 ( 如果是第一次使用 **git**，在使用 **git** 的时候会提示输入用户名和密码，用户名是自己的注册邮箱。 )

```
git remote add origin https://github.com/luxiangqiang/luxiangqiang.github.io.git
```

步骤五：发布 **hexo** 到 **github page** 

```
//等于一次性执行了，清空、刷新、部署三个命令
hexo clean && hexo g && hexo d
```

步骤六：推送到远程仓库（github） 

```
git push origin
```

这里建议创建一个新的分支 **hexo**，用于管理 **hexo** 文件。提交的的时候只提交 **hexo** 网站 **html**、**css** 等源文件。 

创建并切换到新建分支： 

```
git checkout -b hexo
```

将分支推送到远程仓库： 

```
git push origin hexo
```

记得提交以后去 **github** 上把 **hexo** 分支设置默认，以后写文章等都要部署。 文章在 **hexo** 目录下的 **\source\ _posts** 文件夹中，是 **md** 格式，也就是 **Markdown** 格式。 