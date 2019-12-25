---
title: 数据结构与算法之美【Trie 树】
categories:
- 数据结构
tags:
- 数据结构
password:Trie
---

Trie 树实现搜索引擎的搜索关键词提示功能！

<!--more-->



### 一、什么是 Trie 树？

> Trie 树又称‘字典树’。它是一个树形结构。专门处理字符串匹配，用来解决一组字符串集合中快速查找某个字符串的问题。



#### 1、Trie 树的本质

> 利用字符串之间的公共前缀，将重复的前缀合并在一起（根节点不包括任何信息）。从而避免重复存取字符串。

![img](https://static001.geekbang.org/resource/image/28/32/280fbc0bfdef8380fcb632af39e84b32.jpg)



### 二、如何实现一颗 Trie 树

> Trie 树主要有两种操作：
>
> 1) 一是将字符串集合构造成 Trie 树。
>
> 2) Trie 树中查询一个字符串。



#### 1、如何存储 Trie 树

> 最经典的方式就是借助散列表的思想，通过下标与字符——映射的数组，来存储子节点的指针。

```javascript
class TrieNode {
  char data;
  TrieNode children[26];
}
```



#### 2、查找操作

> 通过字符的 ASCLL 码减去 “a” 的 ASCLL 码,迅速找到匹配的子节点指针（所在的下标索引）。

```javascript
public class Trie {
  // 构造结点
  public class TrieNode {
    public char data;
    public TrieNode[] children = new TrieNode[26];
    public boolean isEndingChar = false;
    public TrieNode(char data) {
      this.data = data;
    }
  }  
 
  private TrieNode root = new TrieNode('/'); // 存储无意义字符

  // 往 Trie 树中插入一个字符串
  public void insert(char[] text) {
    TrieNode p = root;
    for (int i = 0; i < text.length; ++i) {
      int index = text[i] - 'a';
      if (p.children[index] == null) {
        TrieNode newNode = new TrieNode(text[i]);
        p.children[index] = newNode;
      }
      p = p.children[index];
    }
    p.isEndingChar = true;
  }

  // 在 Trie 树中查找一个字符串
  public boolean find(char[] pattern) {
    TrieNode p = root;
    for (int i = 0; i < pattern.length; ++i) {
      int index = pattern[i] - 'a';
      if (p.children[index] == null) {
        return false; // 不存在 pattern
      }
      p = p.children[index];
    }
    if (p.isEndingChar == false) return false; // 不能完全匹配，只是前缀
    else return true; // 找到 pattern
  }

}
```



#### 三、Trie 树的性能

> Tire 树有两种操作：
>
> - 构建Trie 树
> - 查找字符串



#### 1、时间复杂度分析

- 构建 Trie 树需要扫描所有的字符串，所以时间复杂度为 O(n)。

- 每次查询的时候，查询的字符串长度为 K，只对比大约 K 个结点，和原来的字符串个数没关系，时间复杂度为O(K)。



#### 2、空间消耗

> 虽然 Trie 可以实现高效的字符串匹配方法，但是 Trie 树正是用了空间换时间的设计思想。

Trie 树的结点在存储元素的时候，有可能要维护像 26 长度的数组，需要存储的结点只有两三个，所以导致了要维护一个很长的数组。如果加上中文、数字等，维护的就更多了，在重复前缀不多的情况下，Trie 不但不能节省空间，还有可能浪费更多的空间。



#### 3、解决空间消耗问题

##### ▉ 方法一:

> 为了能够解决内存消耗问题，可以稍微的牺牲一下查询效率，用其他的数据结构来存储，比如有序数组(二分)、跳表、散列表、红黑树等。

 举例: 有序数组

加入用有序数组来代结点的话，通过二分查找快速的找到匹配的子节点指针，但是插入数据的时候，为了保持数据的有序性，在效率上稍微慢了点。



##### ▉ 方法二:

> 缩点优化。针对于只有一个子节点的节点，而且此节点不是一个串的结束节点，将此节点进行合并，从而可以达到节省内存空间。（确增加了编码的难度）

![img](https://static001.geekbang.org/resource/image/87/11/874d6870e365ec78f57cd1b9d9fbed11.jpg)



### 四、Trie 树的适用条件

> 实际开发中针对一组字符串中查找字符串问题，更倾向于用散列表和红黑树，以为已经有提供的现成的类库。

1、字符串中的字符集不能太大，如果太大，存储空间就会浪费很多，即便可以优化，也要牺牲查询的效率。

2、要求字符串的前缀重合比较多，不然空间消耗变大。

3、实现Trie，要从零开始实现，还要保证没有 bug，其实有点将简单问题复杂化（只有在必要情况这样做）。

4、通过指针串联起来的内存块是不连续的，而 Trie 树中用到了指针，所以对缓存不友好，性能大打折扣。

> **注意：**Trie 树只是不适合于精确的查找，这种问题适合于散列表和红黑树。Trie 树更适合于查找前缀匹配的字符串。



### 五、应用：搜索引擎实现提示功能

> 上边只是简单的原理实现，实际开发中会出现很多的问题需要解决。

1、对于中文或者更复杂的词库，怎么构建 Trie 树？

2、关键词有很多，如果在搜索提示的时候，作为前缀可以匹配的关键词有很多，应该如何展示哪些内容？

3、在用户拼写错误的情况下，怎么给出正确的提示呢？

> Trie 树的其他应用：
>
> - 输入法的自动补全、IDE 代码编辑器的自动补全、浏览器网址的自动补全等。

























