---
title: 数据结构与算法之美【AC 自动机】
categories:
- 数据结构
tags:
- 数据结构
password: lxqwan915816816
copyright: true
---

![](/images/ac自动机.png)

小鹿带你走进数据结构与算法之美的AC 自动机。

<!--more-->



### 一、基于单模式串和 Trie 树实现敏感词过滤

#### 1、多模式串匹配

> 多模式串匹配是在多个模式串中和一个主串之间进行匹配，也就是在一个主串中查找多个模式串。



#### 2、用单模式匹配敏感词缺点

> 用 BF 算法、RK 算法、BM 算法、KMP 算法，还有 Trie 树实现多模式串匹配算法可以实现，但是每个匹过程都要扫描用户输入的内容，整个过程需要扫描很多遍内容，如果用户输入很长，敏感词过多，匹配过程很低效。



#### 3、Trie 树实现敏感词匹配

> 1、我们将敏感词进行预处理，形成 Trie 树结构，如果铭感次更新或者删除了，只需更新一下 Trie 树就可以了。 
>
> 2、当用户输入一个文本内容后，我们把用户输入的内容作为主串，从第一个字符从 Trie 树开始匹配。当遇到叶子节点或者不匹配时，然后从下一个字符开始，重新匹配 Trie 树。



### 二、经典的多模式匹配算法：AC自动机

#### 定义

> AC 自动机算法，全称 Aho-Corasick 算法。AC 自动机实际上就是在 Trie 树之上，加了类似 KMP 的 next 数组，只不过此处构建在树上。



#### 代码

```java
public class AcNode {
  public char data; 
  public AcNode[] children = new AcNode[26]; // 字符集只包含 a~z 这 26 个字符
  public boolean isEndingChar = false; // 结尾字符为 true
  public int length = -1; // 当 isEndingChar=true 时，记录模式串长度
  public AcNode fail; // 失败指针
  public AcNode(char data) {
    this.data = data;
  }
}
```



#### AC 自动机的构建

- 将多模式串构建成树。
- 在 Trie 树上构建失败指针（相当于 KMP 中的失效函数 next 数组）。









