---
title: 小学生都能学会的 Git 之【本地仓库】
categories:
- Git版本控制
tags:
- Git版本控制
copyright: true
---



git 怎么搭建本地仓库？

<!--more-->

### 一、Git 简介



### 二、安装 Git



### 三、配置用户信息

#### 1. 命令来配置用户名和邮箱

> - 有 --global 「所有项目」默认使用此信息。
> - 无 --global 「特定项目」下的配置信息。

```git
//配置用户名
git config --global user.name "xiaolu" 
//配置邮件
git config --global user.email xiaolu@example.com
```

**图示： **

![](C:\Users\10405\Desktop\Git 图示\配置用户名和邮箱1.png)



#### 2、检查是否配置成功

```
//单独检查用户名
git config user.name
//单独检查邮件地址
git config user.email
//查看 git 的全部配置信息
git config --list
```

**图示：**

![](C:\Users\10405\Desktop\Git 图示\检查用户名和邮箱2.png)



### 四、创建 git 项目

> 在本地创建一个文件夹（xiaolu），进入该文件夹，然后初始化该文件夹（仓库）,创建之后就会出现一个隐藏文件 .git ，该文件会记录所有有关 git 操作提交记录和你所进行的 git 全部操作。

**图示：**
![](C:\Users\10405\Desktop\Git 图示\初始化仓库3.png)

**注意：**

> 如果没有发现 .git 隐藏文件，因为我们文件夹设置不显示隐藏文件了，通过在查看的隐藏文件夹中勾选选择显示隐藏文件。

**图示：**

![](C:\Users\10405\Desktop\Git 图示\显示隐藏文件5.png)



### 五、工作区、版本库和暂存区

> 工作区：工作区就是一个文件夹，比如刚刚创建的有 .git 文件夹的命名为 xiaolu 文件夹。
>
> 版本库：工作区有一个 .git 隐藏文件，我们叫做 Git 的版本库，工作区有一个称为 stage 的暂存区。
>
> 暂存区：暂存区就是临时的仓库，存放即将提交的临时文件。



### 六、提交代码

> 我们在文件夹中创建一个命名为 `xiaolu.txt` 的文件（手动创建或命令创建），我们将本地文件夹（仓库）的 `xiaolu.txt `文件提交到 Github 远程仓库中去，将经历以下过程。



#### 1、创建文件夹

```
//命令创建 xiaolu.txt 文件
touch xiaolu.txt
//命令查看在 xiaolu 文件中是否存 xiaolu.txt 文件
ls 
```

**命令图示：**

![](C:\Users\10405\Desktop\Git 图示\命令创建文件7.png)

**可视图示：**

![](C:\Users\10405\Desktop\Git 图示\工作区文件状态6.png)



#### 2、提交过程

> 将本地代码先提交至版本库，等待提交至远程仓库。

```
//1、先将工作区的文件添加到暂存区等待被提交
git add xiaolu.txt
//2、查看缓存区是否存在 xiaolu.txt 文件
git status
//3、将暂存区文件提交至版本库 master 分支（-m 后是提交备注信息）
git commit -m '第一次提交，提交内容为 xiaolu.txt 文件'
//4、显示最近一次提交记录
git show
```

**提交图示：**

![](C:\Users\10405\Desktop\Git 图示\查看缓存区文件8.png)

![](C:\Users\10405\Desktop\Git 图示\git提交9.png)



**提交过程图示：**

![](C:\Users\10405\Desktop\Git 图示\git 提交过程4.png)



#### 3、删除文件

> 删除已经提交至版本库中的文件。新建命名为  file.txt 的文件，提交至版本库，然后进行在版本库中的删除。

**创建 file.txt 提交至版本库：**

![](C:\Users\10405\Desktop\Git 图示\提交file文件10.png)



**删除工作区file文件同时删除版本库file文件：**

```
//1、删除工作区文件
git rm file.txt
//2、查看要删除文件状态
git status
//3、提交删除版本库中的 file 文件
git commit -m '删除file文件'
```

![](C:\Users\10405\Desktop\Git 图示\删除工作区和版本库的文件11.png)

**查看本地所有提交记录：**

```
//4、查看本地所有 git 提交记录
git log
```

![](C:\Users\10405\Desktop\Git 图示\查看本地所有提交记录12.png)



#### 3、拉取版本库代码











