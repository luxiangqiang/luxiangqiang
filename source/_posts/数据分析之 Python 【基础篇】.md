---
title:  数据分析之 Python 【基础篇】
categories:
- python
- 数据分析
tags: 
- python
- 数据分析
---

快速入门 Python 基础!

<!--more-->



#### 1、输入输出

> input 用于 py3.0 的输入，print 用于输出。

```python
name = input("请输入输出的数据")
sum = 100 + 100
print('hello,%s'%name)
print('sum = %d'%sum)
```



#### 2、判断语句（if...else）

```python
# 判断语句
number = input('请输入成绩:')
score = int(number)
if score >= 90:
    print('优秀')
else:
    if score < 60:
        print('不及格')
    else:
        print('及格')
```



#### 3、循环语句

> range(11) 代表从 0 到 10，不包括 11，也相当于 range(0,11)，range 里面还可以增加步长，比如 range(1,11,2) 代表的是 [1,3,5,7,9]。

```python
# 循环语句
sum = 0
for item in range(11):
    sum = sum + item
print(sum)
```



#### 4、while 循环

> while 循环适合循环次数不确定的循环，而 for 循环循环的条件相对确定，适合固定次数的循环。

```python
sum = 0
number = 1
while number < 11:
    sum = sum + number
    number = number + 1
print(sum)
```



#### 5、数据类型：列表、元组、字典、集合

###### ▉ 列表: [] —— 数组

> 列表是 Python 中常用的数据结构，相当于数组，具有增删删改查的功能，我们可以使用 len() 函数获得 lists中元素的个数；使用 append() 在尾部添加元素，使用 insert() 在列表中插入元素，使用 pop() 删除尾部的元素。

```python
# 列表
lists = ['1','2','3']
lists.append('4')
print (lists)
print (len(lists))
lists.insert(0,'xiaolu')
lists.pop()
print (lists)
```



###### ▉ 元组 () —— 只能访问，不能赋值

> 元组 tuple 和 list 非常类似，但是 tuple 一旦初始化就不能修改。因为不能修改所以没有append(), insert() 这样的方法，可以像访问数组一样进行访问，比如 tuples[0]，但不能赋值。

```python
# 元组
tuples = ('xiaolu','小鹿')
print(tuples[0])
```



###### ▉ 字典 {} —— map

> 字典其实就是{key, value},多次对同一个 key 放入 value，后面的值会把前面的值替换，同样有增删改查

```python
# 字典
# 字典定义
score = {'age':22,'name':'xiaolu'}
# 添加数据
score['school'] = '三本'
print(score)
# 删除元素
score.pop('name')
print(score)
# 查看 key 是否存在
print('age' in score)
# 查看 key 对应的值
print(score.get('age'))
# 给一个默认值
print(score.get('school','三本'))
```



###### ▉ 集合: set —— 存储字典的 key 值

> 集合 set 和字典 dictory 类似，不过它只是 key 的集合，不存储 value。同样可以增删查，增加使用 add，删除使用 remove，查询看某个元素是否在这个集合里，使用 in。

```python
# 集合
s = set(['a','b','c'])
# 添加
s.add('d')
# 移除
s.remove('b')
print(s)
print('c' in s)
```



#### 6、引用模块 / 包：import

> 1、import 的本质是路径搜索。import 引用可以是模块 module，或者包 package。
>
> 2、针对 module，实际上是引用一个.py 文件。
>
> 3、而针对 package，可以采用 from … import... 的 方式。
>
> 4、这里实际上是从一个目录中引用模块，这时目录结构中**必须带有一个init.py 文件**。

```python
# 导入一个模块
import model_name
# 导入多个模块
import module_name1,module_name2
# 导入包中指定模块 
from package_name import moudule_name
# 导入包中所有模块 
from package_name import *

```



#### 7、函数：def

> 函数代码块以 def 关键词开头，后接函数标识符名称和圆括号，在圆括号里是传进来的参数，然后通过 return 进行函数结果得反馈。

```python
# 函数
def addone(score):
    return score + 1
print(addone(1))
```































