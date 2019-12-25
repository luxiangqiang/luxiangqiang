---
title: Python 基础之快速入门
categories: 
- Python
tags: 
- Python 基础
---

![](\images\Python基础入门.png)

此篇文章适合于具有一定编程语言基础的开发者快速入门学习 Python。

<!--more-->



# 一、Python 基础语法

## Python 标识符

 **1、以下划线开头的标识符是有特殊意义的。** 

**①** 以单下划线开头 **_foo** 的代表不能直接访问的类属性，需通过类提供的接口进行访问，不能用 **from xxx import *** 而导入； 

**②** 以双下划线开头的 **_ _foo** 代表类的**私有成员**； 

**③** 以双下划线开头和结尾的 __foo__ 代表 **Python** 里特殊方法专用的标识，如 **_ _ init _ _()** 代表类的构造函数。 



# 行和缩进

> 学习 **Python** 与其他语言最大的区别就是，**Python** 的代码块不使用大括号 **{ }** 来控制类，函数以及其他逻辑判断。**python** 最具特色的就是用缩进来写模块。 

缩进的空白数量是可变的，但是·所有代码块语句必须包含相同的缩进空白数量，这个必须严格执行。如下所示： 

```python
if True:
    print "True"
else:
  print "False"
```

以下代码将会执行错误 

```python
if True:
    print "Answer"
    print "True"
else:
    print "Answer"
    # 没有严格缩进，在执行时会报错
  print "False
```

执行以上代码，会出现如下错误提醒： 

```python
  File "test.py", line 10
    print "False"
                ^
IndentationError: unindent does not match any outer indentation level
```

**① IndentationError: unindent does not match any outer indentation level**错误表明，你使用的缩进方式不一致，有的是 tab 键缩进，有的是空格缩进，改为一致即可。 

**②** 如果是 **IndentationError: unexpected indent** 错误, 则 python 编译器是在告诉你"**Hi，老兄，你的文件里格式不对了，可能是 tab 和空格没对齐的问题**"，所有 python 对格式要求非常严格。 

**③** 建议你在每个缩进层次使用 **单个制表符** 或 **两个空格** 或 **四个空格** , 切记不能混用 。



## Python注释

**1、 python **中单行注释采用 **#** 开头。 

**2、python** 中多行注释使用三个单引号 **(''') **或三个双引号 **(""")**。 

```python
'''
这是多行注释，使用单引号。
这是多行注释，使用单引号。
这是多行注释，使用单引号。
'''

"""
这是多行注释，使用双引号。
这是多行注释，使用双引号。
这是多行注释，使用双引号。
"""
```



## Python空行

1、函数之间或类的方法之间用空行分隔，表示一段新的代码的开始。类和函数入口之间也用一行空行分隔，以突出函数入口的开始。

2、空行与代码缩进不同，空行并不是 **Python** 语法的一部分。书写时不插入空行，**Python** 解释器运行也不会出错。但是空行的作用在于分隔两段不同功能或含义的代码，便于日后代码的维护或重构。

**记住：空行也是程序代码的一部分。**



## 等待用户输入

下面的程序执行后就会等待用户输入，按回车键后就会退出： 

```python
raw_input("按下 enter 键退出，其他任意键显示...\n")
```



## Print 输出

**print** 默认输出是换行的，如果要实现不换行需要在变量末尾 **加上逗号** , 

```
x="a"
y="b"
# 换行输出
print x
print y

print '---------'
# 不换行输出
print x,
print y,

# 不换行输出
print x,y
```

以上实例执行结果为： 

```python
a
b
---------
a b a b
```



## 多个语句构成代码组

**①** 缩进相同的一组语句构成一个代码块，我们称之代码组。 

**②** 像 **if、while、def** 和 **class** 这样的复合语句，首行以关键字开始，以冒号 **( : )** 结束，该行之后的一行或多行代码构成代码组。 

如下实例： 

```
if expression : 
   suite 
elif expression :  
   suite  
else :  
   suite 
```



# 二 、Python 变量类型

**1、Python** 中的变量赋值不需要类型声明。 

**2、**每个变量在内存中创建，都包括变量的**标识，名称和数据**这些信息。 

**3、**每个变量在使用前都**必须赋值**，**变量赋值以后该变量才会被创建**。 

例如：

```python
counter = 100 # 赋值整型变量
miles = 1000.0 # 浮点型
name = "John" # 字符串
 
print counter
print miles
print name
```

执行以上程序会输出如下结果： 

```python
100
1000.0
John
```



## 标准数据类型

**Python** 有五个标准的数据类型： 

- **Numbers**（数字）
- **String**（字符串）
- **List**（列表）
- **Tuple**（元组）
- **Dictionary**（字典）



## Python字符串

**1、**字符串或串 **(String)** 是由数字、字母、下划线组成的一串字符。 

**2、python** 的字串列表有 **2** 种取值顺序: 

- 从左到右索引默认 **0** 开始的，最大范围是字符串长度少 **1**
- 从右到左索引默认 **-1** 开始的，最大范围是字符串开头

![img](http://www.runoob.com/wp-content/uploads/2013/11/python-string-slice.png) 



**3、**如果你要实现从字符串中获取一段子字符串的话，可以使用 **[头下标:尾下标]** 来截取相应的字符串，其中下标是从 **0** 开始算起，可以是正数或负数，下标可以为空表示取到头或尾。 

```python
>>> s = 'abcdef'
>>> s[1:5]
'bcde'
```

上面的结果包含了 **s[1]** 的值 **b**，而取到的最大范围不包括**尾下标**，就是 **s[5]** 的值 **f**。 



## Python列表

**1、List**（列表） 是 **Python** 中使用最频繁的数据类型。 

**2、**列表用 **[ ]** 标识，是 **python** 最通用的复合数据类型。 

![img](http://www.runoob.com/wp-content/uploads/2013/11/list_slicing1.png) 

```python
list = [ 'runoob', 786 , 2.23, 'john', 70.2 ]
tinylist = [123, 'john']
 
print list               # 输出完整列表
print list[0]            # 输出列表的第一个元素
print list[1:3]          # 输出第二个至第三个元素 
print list[2:]           # 输出从第三个开始至列表末尾的所有元素
print tinylist * 2       # 输出列表两次
print list + tinylist    # 打印组合的列表
```

以上实例输出结果： 

```python
['runoob', 786, 2.23, 'john', 70.2]
runoob
[786, 2.23]
[2.23, 'john', 70.2]
[123, 'john', 123, 'john']
['runoob', 786, 2.23, 'john', 70.2, 123, 'john']
```



## Python 元组

1、元组是另一个数据类型，类似于**List**（列表）。

2、元组用 **"()"** 标识。内部元素用逗号隔开。但是元组不能二次赋值，相当于只读列表。

实例：

```python
tuple = ( 'runoob', 786 , 2.23, 'john', 70.2 )
tinytuple = (123, 'john')
 
print tuple               # 输出完整元组
print tuple[0]            # 输出元组的第一个元素
print tuple[1:3]          # 输出第二个至第三个的元素 
print tuple[2:]           # 输出从第三个开始至列表末尾的所有元素
print tinytuple * 2       # 输出元组两次
print tuple + tinytuple   # 打印组合的元组
```

以上实例输出结果： 

```python
('runoob', 786, 2.23, 'john', 70.2)
runoob
(786, 2.23)
(2.23, 'john', 70.2)
(123, 'john', 123, 'john')
('runoob', 786, 2.23, 'john', 70.2, 123, 'john')
```

以下是元组无效的，因为元组是不允许更新的。而列表是允许更新的： 

```python
tuple = ( 'runoob', 786 , 2.23, 'john', 70.2 )
list = [ 'runoob', 786 , 2.23, 'john', 70.2 ]
tuple[2] = 1000    # 元组中是非法应用
list[2] = 1000     # 列表中是合法应用
```



## Python 字典

**1、字典(dictionary)**是除列表以外 **python** 之中最灵活的内置数据结构类型。**列表是有序的对象集合，字典是无序的对象集合。**

**2、**两者之间的区别在于：**字典当中的元素是通过键来存取的**，而不是通过偏移存取。

**3、**字典用**"{ }"**标识。字典由索引 **(key)** 和它对应的值 **（value）** 组成。

实例：

```python
dict = {}
dict['one'] = "This is one"
dict[2] = "This is two"
 
tinydict = {'name': 'john','code':6734, 'dept': 'sales'}
 
print dict['one']          # 输出键为'one' 的值
print dict[2]              # 输出键为 2 的值
print tinydict             # 输出完整的字典
print tinydict.keys()      # 输出所有键
print tinydict.values()    # 输出所有值
```

输出结果为： 

```python
This is one
This is two
{'dept': 'sales', 'code': 6734, 'name': 'john'}
['dept', 'code', 'name']
['sales', 6734, 'john']
```



# 三、Python 运算符



## Python逻辑运算符

| 运算符 | 逻辑表达式 | 描述                                                         | 实例                    |
| ------ | ---------- | ------------------------------------------------------------ | ----------------------- |
| and    | x and y    | 布尔"与" - 如果 x 为 False，x and y 返回 False，否则它返回 y 的计算值。 | (a and b) 返回 20。     |
| or     | x or y     | 布尔"或"	- 如果 x 是非 0，它返回 x 的值，否则它返回 y 的计算值。 | (a or b) 返回 10。      |
| not    | not x      | 布尔"非" - 如果 x 为 True，返回 False 。如果 x 为 False，它返回 True。 | not(a and b) 返回 False |



## Python成员运算符

除了以上的一些运算符之外，**Python** 还支持成员运算符，测试实例中包含了一系列的成员，包括字符串，列表或元组。 

| 运算符 | 描述                                                    | 实例                                              |
| ------ | ------------------------------------------------------- | ------------------------------------------------- |
| in     | 如果在指定的序列中找到值返回 True，否则返回 False。     | x 在 y 序列中 , 如果 x 在 y 序列中返回 True。     |
| not in | 如果在指定的序列中没有找到值返回 True，否则返回 False。 | x 不在 y 序列中 , 如果 x 不在 y 序列中返回 True。 |

实例：

```python
a = 10
b = 20
list = [1, 2, 3, 4, 5 ];
 
if ( a in list ):
   print "1 - 变量 a 在给定的列表中 list 中"
else:
   print "1 - 变量 a 不在给定的列表中 list 中"
 
if ( b not in list ):
   print "2 - 变量 b 不在给定的列表中 list 中"
else:
   print "2 - 变量 b 在给定的列表中 list 中"
 
# 修改变量 a 的值
a = 2
if ( a in list ):
   print "3 - 变量 a 在给定的列表中 list 中"
else:
   print "3 - 变量 a 不在给定的列表中 list 中"
```

以上实例输出结果： 

```python
1 - 变量 a 不在给定的列表中 list 中
2 - 变量 b 不在给定的列表中 list 中
3 - 变量 a 在给定的列表中 list 中
```



## Python身份运算符

身份运算符用于比较两个对象的存储单元 。

| 运算符 | 描述                                        | 实例                                                         |
| ------ | ------------------------------------------- | ------------------------------------------------------------ |
| is     | is 是判断两个**标识符**是不是引用自一个对象 | **x is y**, 类似 **id(x) == id(y)** , 如果引用的是同一个对象则返回 True，否则返回 False |
| is not | is not 是判断两个标识符是不是引用自不同对象 | **x is not y** ， 类似 **id(a) != id(b)**。如果引用的不是同一个对象则返回结果 True，否则返回 False。 |

实例：

```python
a = 20
b = 20
 
if ( a is b ):
   print "1 - a 和 b 有相同的标识"
else:
   print "1 - a 和 b 没有相同的标识"
 
if ( a is not b ):
   print "2 - a 和 b 没有相同的标识"
else:
   print "2 - a 和 b 有相同的标识"
 
# 修改变量 b 的值
b = 30
if ( a is b ):
   print "3 - a 和 b 有相同的标识"
else:
   print "3 - a 和 b 没有相同的标识"
 
if ( a is not b ):
   print "4 - a 和 b 没有相同的标识"
else:
   print "4 - a 和 b 有相同的标识"
```

以上实例输出结果： 

```python
1 - a 和 b 有相同的标识
2 - a 和 b 有相同的标识
3 - a 和 b 没有相同的标识
4 - a 和 b 没有相同的标识
```

> is 与 == 区别：
>
> is 用于判断两个变量引用对象是否为同一个， == 用于判断引用变量的值是否相等。



# 四、python 条件语句



**1、Python** 编程中 **if** 语句用于控制程序的执行，基本形式为： 

```python
if 判断条件：
    执行语句……
else：
    执行语句……
```

**2、**当判断条件为多个值时，可以使用以下形式： 

```python
if 判断条件1:
    执行语句1……
elif 判断条件2:
    执行语句2……
elif 判断条件3:
    执行语句3……
else:
    执行语句4……
```

**3、**由于 **python** 并不支持 **switch** 语句，所以多个条件判断，只能用 **elif** 来实现，如果判断需要多个条件需同时判断时，可以使用 **or** （或），表示两个条件有一个成立时判断条件成功；使用 **and** （与）时，表示只有两个条件同时成立的情况下，判断条件才成功。 



# 五、Python 循环语句

Python提供了for循环和while循环（在Python中没有do..while循环）: 

| 循环类型   | 描述                                                   |
| ---------- | ------------------------------------------------------ |
| while 循环 | 在给定的判断条件为 true 时执行循环体，否则退出循环体。 |
| for 循环   | 重复执行语句                                           |
| 嵌套循环   | 你可以在while循环体中嵌套for循环                       |

# 1、while 循环

```python
while 判断条件：
    执行语句……
```

![img](http://www.runoob.com/wp-content/uploads/2013/11/loop-over-python-list-animation.gif) 



实例：

```python
count = 0
while (count < 9):
   print 'The count is:', count
   count = count + 1
 
print "Good bye!"
```

以上代码执行输出结果: 

```python
The count is: 0
The count is: 1
The count is: 2
The count is: 3
The count is: 4
The count is: 5
The count is: 6
The count is: 7
The count is: 8
Good bye!
```



### 循环使用 else 语句

在 **python** 中，**while … else** 在循环条件为 **false** 时执行 **else** 语句块： 

```python
count = 0
while count < 5:
   print count, " is  less than 5"
   count = count + 1
else:
   print count, " is not less than 5"
```

以上实例输出结果为： 

```python
0 is less than 5
1 is less than 5
2 is less than 5
3 is less than 5
4 is less than 5
5 is not less than 5
```



# 2、for 循环语句

**Python for** 循环可以遍历任何序列的项目，如一个列表或者一个字符串。 

**语法：** 

```python
for iterating_var in sequence:
   statements(s)
```

实例：

```python
for letter in 'Python':     # 第一个实例
   print '当前字母 :', letter
 
fruits = ['banana', 'apple',  'mango']
for fruit in fruits:        # 第二个实例
   print '当前水果 :', fruit
 
print "Good bye!"
```

以上实例输出结果: 

```python
当前字母 : P
当前字母 : y
当前字母 : t
当前字母 : h
当前字母 : o
当前字母 : n
当前水果 : banana
当前水果 : apple
当前水果 : mango
Good bye!
```

### 通过序列索引迭代

另外一种执行循环的遍历方式是通过索引，如下实例： 

```python
fruits = ['banana', 'apple',  'mango']
for index in range(len(fruits)):
   print '当前水果 :', fruits[index]
 
print "Good bye!"
```

以上实例输出结果： 

```python
当前水果 : banana
当前水果 : apple
当前水果 : mango
Good bye!
```

以上实例我们使用了内置函数 **len()** 和 **range()**,函数 len() 返回列表的长度，即元素的个数。 **range** 返回一个序列的数。 

### 循环使用 else 语句

在 **python** 中，**for … else** 表示这样的意思，**for** 中的语句和普通的没有区别，**else** 中的语句会在循环正常执行完（即 **for** 不是通过 **break** 跳出而中断的）的情况下执行，**while … else** 也是一样 

实例：

```python
for num in range(10,20):  # 迭代 10 到 20 之间的数字
   for i in range(2,num): # 根据因子迭代
      if num%i == 0:      # 确定第一个因子
         j=num/i          # 计算第二个因子
         print '%d 等于 %d * %d' % (num,i,j)
         break            # 跳出当前循环
   else:                  # 循环的 else 部分
      print num, '是一个质数'
```

以上实例输出结果： 

```python
10 等于 2 * 5
11 是一个质数
12 等于 2 * 6
13 是一个质数
14 等于 2 * 7
15 等于 3 * 5
16 等于 2 * 8
17 是一个质数
18 等于 2 * 9
19 是一个质数
```



# 六、Python pass 语句

**1、Python** **pass** 是空语句，是为了保持程序结构的完整性。

**2、pass** 不做任何事情，一般用做占位语句。

实例：

```python
# 输出 Python 的每个字母
for letter in 'Python':
   if letter == 'h':
      pass
      print '这是 pass 块'
   print '当前字母 :', letter

print "Good bye!"
```

以上实例执行结果： 

```python
当前字母 : P
当前字母 : y
当前字母 : t
这是 pass 块
当前字母 : h
当前字母 : o
当前字母 : n
Good bye!
```



# 七、Python 列表(List)

**1、**序列是 **Python** 中最基本的数据结构。序列中的每个元素都分配一个数字 - 它的位置，或索引，第一个索引是**0**，第二个索引是 **1**，依此类推。 

**2、**创建一个列表，只要把逗号分隔的不同的数据项使用方括号括起来即可。如下所示： 

```python
list1 = ['physics', 'chemistry', 1997, 2000]
list2 = [1, 2, 3, 4, 5 ]
list3 = ["a", "b", "c", "d"]
```

与字符串的索引一样，列表索引从 **0** 开始。列表可以进行截取、组合等。 

## 访问列表中的值

使用下标索引来访问列表中的值，同样你也可以使用方括号的形式截取字符，如下所示： 

```python
list1 = ['physics', 'chemistry', 1997, 2000]
list2 = [1, 2, 3, 4, 5, 6, 7 ]
 
print "list1[0]: ", list1[0]
print "list2[1:5]: ", list2[1:5]
```

以上实例输出结果： 

```python
list1[0]:  physics
list2[1:5]:  [2, 3, 4, 5]
```

## 更新列表

你可以对列表的数据项进行**修改或更新**，你也可以使用 **append()** 方法来添加列表项，如下所示： 

```python
list = []          ## 空列表
list.append('Google')   ## 使用 append() 添加元素
list.append('Runoob')
print list
```

以上实例输出结果 ：

```python
['Google', 'Runoob']
```

## 删除列表元素

可以使用 **del** 语句来删除列表的元素，如下实例： 

```python
list1 = ['physics', 'chemistry', 1997, 2000]
 
print list1
del list1[2]
print "After deleting value at index 2 : "
print list1
```

以上实例输出结果： 

```python
['physics', 'chemistry', 1997, 2000]
After deleting value at index 2 :
['physics', 'chemistry', 2000]
```

## Python 列表脚本操作符

| Python 表达式                | 结果                         | 描述                 |
| ---------------------------- | ---------------------------- | -------------------- |
| len([1, 2, 3])               | 3                            | 长度                 |
| [1, 2, 3] + [4, 5, 6]        | [1, 2, 3, 4, 5, 6]           | 组合                 |
| ['Hi!'] * 4                  | ['Hi!', 'Hi!', 'Hi!', 'Hi!'] | 重复                 |
| 3 in [1, 2, 3]               | True                         | 元素是否存在于列表中 |
| for x in [1, 2, 3]: print x, | 1 2 3                        | 迭代                 |



# 八、Python 元组

**1、Python** 的元组与列表类似，不同之处在于元组的**元素不能修改**。 

**2、如下实例：** 

```python
tup1 = ('physics', 'chemistry', 1997, 2000)
tup2 = (1, 2, 3, 4, 5 )
tup3 = "a", "b", "c", "d"
```

创建空元组 

```
tup1 = ()
```

元组中只包含一个元素时，需要在元素后面添加逗号 

```python
tup1 = (50,)
```



## 访问元组

元组可以使用下标索引来访问元组中的值，如下实例: 

```python
tup1 = ('physics', 'chemistry', 1997, 2000)
tup2 = (1, 2, 3, 4, 5, 6, 7 )
 
print "tup1[0]: ", tup1[0]
print "tup2[1:5]: ", tup2[1:5]
```

以上实例输出结果 :

```
tup1[0]:  physics
tup2[1:5]:  (2, 3, 4, 5)
```



## 修改元组

**元组中的元素值是不允许修改的**，但我们可以对元组进行连接组合，如下实例: 

```python
tup1 = (12, 34.56)
tup2 = ('abc', 'xyz')
 
# 以下修改元组元素操作是非法的。
# tup1[0] = 100
 
# 创建一个新的元组
tup3 = tup1 + tup2
print tup3
```

以上实例输出结果： 

```python
(12, 34.56, 'abc', 'xyz')
```



## 删除元组

元组中的元素值是不允许删除的，但我们可以使用del语句来删除整个元组，如下实例: 

```python
tup = ('physics', 'chemistry', 1997, 2000)
 
print tup
del tup
print "After deleting tup : "
print tup
```

以上实例元组被删除后，输出变量会有异常信息，输出如下所示： 

```python
('physics', 'chemistry', 1997, 2000)
After deleting tup :
Traceback (most recent call last):
  File "test.py", line 9, in <module>
    print tup
NameError: name 'tup' is not defined
```



## 元组运算符

与字符串一样，元组之间可以使用 **+** 号和 ***** 号进行运算。这就意味着他们可以组合和复制，运算后会生成一个新的元组。 

| Python 表达式                | 结果                         | 描述         |
| ---------------------------- | ---------------------------- | ------------ |
| len((1, 2, 3))               | 3                            | 计算元素个数 |
| (1, 2, 3) + (4, 5, 6)        | (1, 2, 3, 4, 5, 6)           | 连接         |
| ('Hi!',) * 4                 | ('Hi!', 'Hi!', 'Hi!', 'Hi!') | 复制         |
| 3 in (1, 2, 3)               | True                         | 元素是否存在 |
| for x in (1, 2, 3): print x, | 1 2 3                        | 迭代         |



## 无关闭分隔符

任意无符号的对象，以逗号隔开，默认为元组，如下实例： 

```python
print 'abc', -4.24e93, 18+6.6j, 'xyz'
x, y = 1, 2
print "Value of x , y : ", x,y
```

以上实例运行结果： 

```
abc -4.24e+93 (18+6.6j) xyz
Value of x , y : 1 2
```



# 九、Python 字典(Dictionary)

**1**、字典是另一种可变容器模型，且可存储任意类型对象。

**2、**字典的每个键值 **key=>value** 对用冒号 : 分割，每个键值对之间用逗号 , 分割，整个字典包括在花括号 **{}** 中 ,格式如下所示：

```python
d = {key1 : value1, key2 : value2 }
```

键一般是唯一的，如果重复最后的一个键值对会替换前面的，值不需要唯一。 

```python
>>>dict = {'a': 1, 'b': 2, 'b': '3'};
>>> dict['b']
'3'
>>> dict
{'a': 1, 'b': '3'}
```

值可以取任何数据类型，但键必须是不可变的，如字符串，数字或元组。

一个简单的字典实例:

```python
dict = {'Alice': '2341', 'Beth': '9102', 'Cecil': '3258'}
```

也可如此创建字典： 

```python
dict1 = { 'abc': 456 };
dict2 = { 'abc': 123, 98.6: 37 };
```



## 访问字典里的值

把相应的键放入熟悉的方括弧，如下实例: 

```python
dict = {'Name': 'Zara', 'Age': 7, 'Class': 'First'};
 
print "dict['Name']: ", dict['Name'];
print "dict['Age']: ", dict['Age'];
```

以上实例输出结果： 

```
dict['Name']:  Zara
dict['Age']:  7
```



## 修改字典

向字典添加新内容的方法是增加新的键/值对，修改或删除已有键/值对如下实例: 

```python
dict = {'Name': 'Zara', 'Age': 7, 'Class': 'First'};
 
dict['Age'] = 8; # update existing entry
dict['School'] = "DPS School"; # Add new entry
 
 
print "dict['Age']: ", dict['Age'];
print "dict['School']: ", dict['School'];
```

以上实例输出结果： 

```
dict['Age']:  8
dict['School']:  DPS School
```



## 删除字典元素

**1、**能删单一的元素也能清空字典，清空只需一项操作。

**2、**显示删除一个字典用del命令，如下实例：

```python
dict = {'Name': 'Zara', 'Age': 7, 'Class': 'First'};
 
del dict['Name']; # 删除键是'Name'的条目
dict.clear();     # 清空词典所有条目
del dict ;        # 删除词典
 
print "dict['Age']: ", dict['Age'];
print "dict['School']: ", dict['School'];
```

但这会引发一个异常，因为用del后字典不再存在： 

```python
dict['Age']:
Traceback (most recent call last):
  File "test.py", line 8, in <module>
    print "dict['Age']: ", dict['Age'];
TypeError: 'type' object is unsubscriptable
```

### 字典键的特性

字典值可以没有限制地取任何 **python** 对象，既可以是标准的对象，也可以是用户定义的，但键不行。

**两个重要的点需要记住：**

1）不允许同一个键出现两次。创建时如果同一个键被赋值两次，后一个值会被记住，如下实例：

```python
dict = {'Name': 'Zara', 'Age': 7, 'Name': 'Manni'};
 
print "dict['Name']: ", dict['Name'];
```

以上实例输出结果： 

```python
dict['Name']:  Manni
```

2）键必须不可变，所以可以用数字，字符串或元组充当，**所以用列表就不行**，如下实例： 

```python
dict = {['Name']: 'Zara', 'Age': 7};
 
print "dict['Name']: ", dict['Name'];
```

以上实例输出结果： 

```python
Traceback (most recent call last):
  File "test.py", line 3, in <module>
    dict = {['Name']: 'Zara', 'Age': 7};
TypeError: list objects are unhashable
```



# 十、Python 函数

1、函数能提高应用的模块性，和代码的重复利用率。你已经知道 **Python** 提供了许多内建函数，比如 **print()**。但你也可以自己创建函数，这被叫做用户自定义函数。



## 定义一个函数

你可以定义一个由自己想要功能的函数，以下是简单的规则：

- 函数代码块以 **def** 关键词开头，后接函数**标识符名称**和圆括号**()**。
- 任何传入参数和自变量必须放在圆括号中间。圆括号之间可以用于定义参数。
- 函数的第一行语句可以选择性地使用文档字符串—用于存放函数说明。
- 函数内容以**冒号起始**，**并且缩进**。
- **return [表达式]** 结束函数，选择性地返回一个值给调用方。不带表达式的 **return** 相当于返回 **None**。

### 语法

```python
def functionname( parameters ):
   "函数_文档字符串"
   function_suite
   return [expression]
```

默认情况下，参数值和参数名称是按函数声明中定义的顺序匹配起来的。 

### 实例

```python
def printme( str ):
   "打印传入的字符串到标准显示设备上"
   print str
   return
```

## 函数调用

这个函数的基本结构完成以后，你可以通过另一个函数调用执行，也可以直接从 **Python** 提示符执行。

如下实例调用了 **printme（）**函数：

```python
# 定义函数
def printme( str ):
   "打印任何传入的字符串"
   print str;
   return;
 
# 调用函数
printme("我要调用用户自定义函数!");
printme("再次调用同一函数");
```

以上实例输出结果： 

```
我要调用用户自定义函数!
再次调用同一函数
```



## 参数传递

**在 python 中，类型属于对象，变量是没有类型的：** 

```python
a=[1,2,3]
 
a="Runoob"
```

**以上代码中，[1,2,3] 是 List 类型，"Runoob" 是 String 类型，而变量 a 是没有类型，她仅仅是一个对象的引用（一个指针），可以是 List 类型对象，也可以指向 String 类型对象。** 



### 可更改(mutable)与不可更改(immutable)对象

在 **python** 中，**strings**, **tuples**, 和 **numbers** 是不可更改的对象，而 **list,dict** 等则是可以修改的对象。

- **不可变类型：**变量赋值 **a=5** 后再赋值 **a=10**，这里实际是新生成一个 **int** 值对象 **10**，再让 **a** 指向它，而 **5** 被丢弃，不是改变 **a** 的值，相当于新生成了 **a**。
- **可变类型：**变量赋值 **la=[1,2,3,4]** 后再赋值 **la[2]=5** 则是将 **list la** 的第三个元素值更改，本身 **la** 没有动，只是其内部的一部分值被修改了。

**python** 函数的参数传递：

- **不可变类型：**类似 **c++**  的值传递，如 **整数、字符串、元组**。如 **fun（a）**，传递的只是 **a** 的值，没有影响 **a** 对象本身。比如在 **fun（a）**内部修改 **a** 的值，只是修改另一个复制的对象，不会影响 **a** 本身。
- **可变类型：**类似 **c++** 的引用传递，如 **列表，字典**。如 **fun（la**），则是将 **la** 真正的传过去，修改后 **fun** 外部的 **la** 也会受影响

**python** 中一切都是对象，严格意义我们不能说值传递还是引用传递，我们应该说传**不可变对象和传可变对象。**

### python 传不可变对象实例

```python
def ChangeInt( a ):
    a = 10
 
b = 2
ChangeInt(b)
print b # 结果是 2
```

实例中有 **int** 对象 **2**，指向它的变量是 **b**，在传递给 **ChangeInt** 函数时，按传值的方式复制了变量 **b**，**a** 和 **b** 都指向了同一个 **Int** 对象，在 **a=10** 时，则新生成一个 **int** 值对象 **10**，并让 **a** 指向它。 

### 传可变对象实例

```python
# 可写函数说明
def changeme( mylist ):
   "修改传入的列表"
   mylist.append([1,2,3,4]);
   print "函数内取值: ", mylist
   return
 
# 调用changeme函数
mylist = [10,20,30];
changeme( mylist );
print "函数外取值: ", mylist
```

实例中传入函数的和在末尾添加新内容的对象用的是同一个引用，故输出结果如下： 

```python
函数内取值:  [10, 20, 30, [1, 2, 3, 4]]
函数外取值:  [10, 20, 30, [1, 2, 3, 4]]
```

### 参数

以下是调用函数时可使用的正式参数类型：

- 必备参数
- 关键字参数
- 默认参数
- 不定长参数

### 必备参数

必备参数须以正确的顺序传入函数。调用时的数量必须和声明时的一样。 

```python
#可写函数说明
def printme( str ):
   "打印任何传入的字符串"
   print str;
   return;
 
#调用printme函数
printme();
```

以上实例输出结果： 

```python
Traceback (most recent call last):
  File "test.py", line 11, in <module>
    printme();
TypeError: printme() takes exactly 1 argument (0 given)
```

### 关键字参数

1、关键字参数和函数调用关系紧密，函数调用使用关键字参数来确定传入的参数值。

2、使用关键字参数允许函数调用时参数的顺序与声明时不一致，因为 **Python** 解释器能够用**参数名**匹配参数值。

以下实例在函数 **printme()** 调用时使用参数名：

```python
#可写函数说明
def printme( str ):
   "打印任何传入的字符串"
   print str;
   return;
 
#调用printme函数
printme( str = "My string");
```

以上实例输出结果： 

```
My string
```

下例能将关键字参数顺序不重要展示得更清楚： 

```python
#可写函数说明
def printinfo( name, age ):
   "打印任何传入的字符串"
   print "Name: ", name;
   print "Age ", age;
   return;
 
#调用printinfo函数
printinfo( age=50, name="miki" );
```

以上实例输出结果： 

```python
Name:  miki
Age  50
```

### 缺省参数

调用函数时，缺省参数的值如果没有传入，则被认为是默认值。下例会打印默认的 **age**，如果 **age** 没有被传入： 

```python
#可写函数说明
def printinfo( name, age = 35 ):
   "打印任何传入的字符串"
   print "Name: ", name;
   print "Age ", age;
   return;
 
#调用printinfo函数
printinfo( age=50, name="miki" );
printinfo( name="miki" );
```

以上实例输出结果： 

```python
Name:  miki
Age  50
Name:  miki
Age  35
```

### 不定长参数

你可能需要一个函数能处理比当初声明时更多的参数。这些参数叫做不定长参数，和上述2种参数不同，声明时不会命名。基本语法如下： 

```python
def functionname([formal_args,] *var_args_tuple ):
   "函数_文档字符串"
   function_suite
   return [expression]
```

加了星号（*）的变量名会存放所有未命名的变量参数。不定长参数实例如下： 

```python
# 可写函数说明
def printinfo( arg1, *vartuple ):
   "打印任何传入的参数"
   print "输出: "
   print arg1
   for var in vartuple:
      print var
   return;
 
# 调用printinfo 函数
printinfo( 10 );
printinfo( 70, 60, 50 );
```

以上实例输出结果： 

```
输出:
10
输出:
70
60
50
```

### 匿名函数

**python** 使用 **lambda** 来创建匿名函数。

- **lambda** 只是一个表达式，函数体比 **def** 简单很多。
- **lambda **的主体是一个表达式，而不是一个代码块。仅仅能在 **lambda** 表达式中封装有限的逻辑进去。
- **lambda **函数拥有自己的命名空间，且不能访问自有参数列表之外或全局命名空间里的参数。
- 虽然 **lambda** 函数看起来只能写一行，却不等同于 **C** 或 **C++** 的内联函数，后者的目的是调用小函数时不占用栈内存从而增加运行效率。

### 语法

**lambda** 函数的语法只包含一个语句，如下： 

```python
lambda [arg1 [,arg2,.....argn]]:expression
```

如下实例： 

```python
# 可写函数说明
sum = lambda arg1, arg2: arg1 + arg2;
 
# 调用sum函数
print "相加后的值为 : ", sum( 10, 20 )
print "相加后的值为 : ", sum( 20, 20 )
```

以上实例输出结果： 

```
相加后的值为 :  30
相加后的值为 :  40
```

### return 语句

**return** 语句[表达式]退出函数，选择性地向调用方返回一个表达式。不带参数值的 **return** 语句返回 **None**。之前的例子都没有示范如何返回数值，下例便告诉你怎么做： 

```python
# 可写函数说明
def sum( arg1, arg2 ):
   # 返回2个参数的和."
   total = arg1 + arg2
   print "函数内 : ", total
   return total;
 
# 调用sum函数
total = sum( 10, 20 );
```

以上实例输出结果： 

```python
函数内 :  30
```

### 变量作用域

一个程序的所有的变量并不是在哪个位置都可以访问的。访问权限决定于这个变量是在哪里赋值的。

变量的作用域决定了在哪一部分程序你可以访问哪个特定的变量名称。两种最基本的变量作用域如下：

- **全局变量**
- **局部变量**

### 全局变量和局部变量

**1、**定义在函数内部的变量拥有一个局部作用域，定义在函数外的拥有全局作用域。

**2、**局部变量只能在其被声明的函数内部访问，而全局变量可以在整个程序范围内访问。调用函数时，所有在函数内声明的变量名称都将被加入到作用域中。如下实例：

```python
total = 0; # 这是一个全局变量
# 可写函数说明
def sum( arg1, arg2 ):
   #返回2个参数的和."
   total = arg1 + arg2; # total在这里是局部变量.
   print "函数内是局部变量 : ", total
   return total;
 
#调用sum函数
sum( 10, 20 );
print "函数外是全局变量 : ", total
```

以上实例输出结果： 

```python
函数内是局部变量 :  30
函数外是全局变量 :  0
```



# 十一、Python 文件I/O



## 读取键盘输入

**Python** 提供了两个内置函数从标准输入读入一行文本，默认的标准输入是键盘。如下： 

- **raw_input**
- **input**

### raw_input函数

**raw_input([prompt])** 函数从标准输入读取一个行，并返回一个字符串（去掉结尾的换行符）： 

```python
str = raw_input("请输入：")
print "你输入的内容是: ", str
```

这将提示你输入任意字符串，然后在屏幕上显示相同的字符串。当我输入"Hello Python！"，它的输出如下： 

```python
请输入：Hello Python！
你输入的内容是:  Hello Python！
```

### input函数

**nput([prompt])** 函数和 **raw_input([prompt])** 函数基本类似，但是 **input** 可以接收一个 **Python** 表达式作为输入，并将运算结果返回。 

```python
str = input("请输入：")
print "你输入的内容是: ", str
```

这会产生如下的对应着输入的结果： 

```python
请输入：[x*5 for x in range(2,10,2)]
你输入的内容是:  [10, 20, 30, 40]
```

### 打开和关闭文件

**Python** 提供了必要的函数和方法进行默认情况下的文件基本操作。你可以用 **file** 对象做大部分的文件操作 

### open 函数

你必须先用 **Python** 内置的 **open()** 函数打开一个文件，创建一个 **file** 对象，相关的方法才可以调用它进行读写。

语法：

```python
file object = open(file_name [, access_mode][, buffering])
```

各个参数的细节如下：

- **file_name：file_name** 变量是一个包含了你要访问的文件名称的字符串值。
- **access_mode：access_mode** 决定了打开文件的模式：只读，写入，追加等。所有可取值见如下的完全列表。这个参数是非强制的，默认文件访问模式为只读 **(r)**。
- **buffering** :如果 **buffering** 的值被设为 **0**，就不会有寄存。如果 **buffering** 的值取 **1**，访问文件时会寄存行。如果将 **buffering** 的值设为大于 **1** 的整数，表明了这就是的寄存区的缓冲大小。如果取负值，寄存区的缓冲大小则为系统默认。

不同模式打开文件的完全列表： 

| 模式 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| r    | 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。 |
| rb   | 以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。一般用于非文本文件如图片等。 |
| r+   | 打开一个文件用于读写。文件指针将会放在文件的开头。           |
| rb+  | 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。一般用于非文本文件如图片等。 |
| w    | 打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
| wb   | 以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如图片等。 |
| w+   | 打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
| wb+  | 以二进制格式打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如图片等。 |
| a    | 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| ab   | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
| a+   | 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。 |
| ab+  | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。 |

### File对象的属性

一个文件被打开后，你有一个 **file** 对象，你可以得到有关该文件的各种信息。

以下是和 **file** 对象相关的所有属性的列表：

| 属性           | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| file.closed    | 返回 true 如果文件已被关闭，否则返回 false。                 |
| file.mode      | 返回被打开文件的访问模式。                                   |
| file.name      | 返回文件的名称。                                             |
| file.softspace | 如果用 print 输出后，必须跟一个空格符，则返回 false。否则返回 true。 |

如下实例： 

```python
# 打开一个文件
fo = open("foo.txt", "w")
print "文件名: ", fo.name
print "是否已关闭 : ", fo.closed
print "访问模式 : ", fo.mode
print "末尾是否强制加空格 : ", fo.softspace
```

以上实例输出结果： 

```python
文件名:  foo.txt
是否已关闭 :  False
访问模式 :  w
末尾是否强制加空格 :  0
```

### close()方法

语法：

```
fileObject.close()
```

### write()方法

**1、write()** 方法可将任何字符串写入一个打开的文件。需要重点注意的是，**Python** 字符串可以是二进制数据，而不是仅仅是文字。

**2、write()** 方法不会在字符串的结尾添加换行符**('\n')：**

语法：

```python
fileObject.write(string)
```

在这里，被传递的参数是要写入到已打开文件的内容。 

例子： 

```python
# 打开一个文件
fo = open("foo.txt", "w")
fo.write( "www.runoob.com!\nVery good site!\n")
 
# 关闭打开的文件
fo.close()
```

上述方法会创建foo.txt文件，并将收到的内容写入该文件，并最终关闭文件。如果你打开这个文件，将看到以下内容:

```python
$ cat foo.txt 
www.runoob.com!
Very good site!
```

### read()方法

****方法从一个打开的文件中读取一个字符串。需要重点注意的是，**Python** 字符串可以是二进制数据，而不是仅仅是文字。

语法：

```
fileObject.read([count])
```

在这里，被传递的参数是要从已打开文件中读取的字节计数。该方法从文件的开头开始读入，如果没有传入**count**，它会尝试尽可能多地读取更多的内容，很可能是直到文件的末尾。

例子：

```python
# 打开一个文件
fo = open("foo.txt", "r+")
str = fo.read(10)
print "读取的字符串是 : ", str
# 关闭打开的文件
fo.close()
```

以上实例输出结果：

```python
读取的字符串是 :  www.runoob
```

### 文件定位

**tell()**  方法告诉你文件内的当前位置 ,**seek（offset [,from]）**方法改变当前文件的位置。**Offset** 变量表示要移动的字节数。**From** 变量指定开始移动字节的参考位置。 

例子：

```python
# 打开一个文件
fo = open("foo.txt", "r+")
str = fo.read(10)
print "读取的字符串是 : ", str
 
# 查找当前位置
position = fo.tell()
print "当前文件位置 : ", position
 
# 把指针再次重新定位到文件开头
position = fo.seek(0, 0)
str = fo.read(10)
print "重新读取字符串 : ", str
# 关闭打开的文件
fo.close()
```

以上实例输出结果：

```
读取的字符串是 :  www.runoob
当前文件位置 :  10
重新读取字符串 :  www.runoob
```



#  Python 异常处理

**python** 提供了两个非常重要的功能来处理 **python** 程序在运行中出现的异常和错误。你可以使用该功能来调试**python** 程序。 

- **异常处理。**
- **断言(Assertions)。**



## python标准异常

| 异常名称                  | 描述                                               |
| ------------------------- | -------------------------------------------------- |
|                           |                                                    |
| BaseException             | 所有异常的基类                                     |
| SystemExit                | 解释器请求退出                                     |
| KeyboardInterrupt         | 用户中断执行(通常是输入^C)                         |
| Exception                 | 常规错误的基类                                     |
| StopIteration             | 迭代器没有更多的值                                 |
| GeneratorExit             | 生成器(generator)发生异常来通知退出                |
| StandardError             | 所有的内建标准异常的基类                           |
| ArithmeticError           | 所有数值计算错误的基类                             |
| FloatingPointError        | 浮点计算错误                                       |
| OverflowError             | 数值运算超出最大限制                               |
| ZeroDivisionError         | 除(或取模)零 (所有数据类型)                        |
| AssertionError            | 断言语句失败                                       |
| AttributeError            | 对象没有这个属性                                   |
| EOFError                  | 没有内建输入,到达EOF 标记                          |
| EnvironmentError          | 操作系统错误的基类                                 |
| IOError                   | 输入/输出操作失败                                  |
| OSError                   | 操作系统错误                                       |
| WindowsError              | 系统调用失败                                       |
| ImportError               | 导入模块/对象失败                                  |
| LookupError               | 无效数据查询的基类                                 |
| IndexError                | 序列中没有此索引(index)                            |
| KeyError                  | 映射中没有这个键                                   |
| MemoryError               | 内存溢出错误(对于Python 解释器不是致命的)          |
| NameError                 | 未声明/初始化对象 (没有属性)                       |
| UnboundLocalError         | 访问未初始化的本地变量                             |
| ReferenceError            | 弱引用(Weak reference)试图访问已经垃圾回收了的对象 |
| RuntimeError              | 一般的运行时错误                                   |
| NotImplementedError       | 尚未实现的方法                                     |
| SyntaxError               | Python 语法错误                                    |
| IndentationError          | 缩进错误                                           |
| TabError                  | Tab 和空格混用                                     |
| SystemError               | 一般的解释器系统错误                               |
| TypeError                 | 对类型无效的操作                                   |
| ValueError                | 传入无效的参数                                     |
| UnicodeError              | Unicode 相关的错误                                 |
| UnicodeDecodeError        | Unicode 解码时的错误                               |
| UnicodeEncodeError        | Unicode 编码时错误                                 |
| UnicodeTranslateError     | Unicode 转换时错误                                 |
| Warning                   | 警告的基类                                         |
| DeprecationWarning        | 关于被弃用的特征的警告                             |
| FutureWarning             | 关于构造将来语义会有改变的警告                     |
| OverflowWarning           | 旧的关于自动提升为长整型(long)的警告               |
| PendingDeprecationWarning | 关于特性将会被废弃的警告                           |
| RuntimeWarning            | 可疑的运行时行为(runtime behavior)的警告           |
| SyntaxWarning             | 可疑的语法的警告                                   |
| UserWarning               | 用户代码生成的警告                                 |

### 异常处理

**1、**捕捉异常可以使用 **try/except** 语句。

**2、try/except** 语句用来检测 **try** 语句块中的错误，从而让 **except** 语句捕获异常信息并处理。

**3、**如果你不想在异常发生时结束你的程序，只需在 **try** 里捕获它。

语法：

```python
try:
<语句>        #运行别的代码
except <名字>：
<语句>        #如果在try部份引发了'name'异常
except <名字>，<数据>:
<语句>        #如果引发了'name'异常，获得附加的数据
else:
<语句>        #如果没有异常发生
```

try 的工作原理是，当开始一个 try 语句后，python 就在当前程序的上下文中作标记，这样当异常出现时就可以回到这里，try 子句先执行，接下来会发生什么依赖于执行时是否出现异常。

- 如果当 try 后的语句执行时发生异常，python 就跳回到 try 并执行第一个匹配该异常的 except 子句，异常处理完毕，控制流就通过整个 try 语句（除非在处理异常时又引发新的异常）。
- 如果在 try 后的语句里发生了异常，却没有匹配的 except 子句，异常将被递交到上层的 try，或者到程序的最上层（这样将结束程序，并打印缺省的出错信息）。
- 如果在 try 子句执行时没有发生异常，python 将执行 else 语句后的语句（如果有 else 的话），然后控制流通过整个 try 语句。

### 实例1

下面是简单的例子，它打开一个文件，在该文件中的内容写入内容，且并未发生异常： 

```python
try:
    fh = open("testfile", "w")
    fh.write("这是一个测试文件，用于测试异常!!")
except IOError:
    print "Error: 没有找到文件或读取文件失败"
else:
    print "内容写入文件成功"
    fh.close()
```

以上程序输出结果： 

```python
$ python test.py 
内容写入文件成功
$ cat testfile       # 查看写入的内容
这是一个测试文件，用于测试异常!!
```

### 实例2

下面是简单的例子，它打开一个文件，在该文件中的内容写入内容，但文件没有写入权限，发生了异常： 

```python
try:
    fh = open("testfile", "w")
    fh.write("这是一个测试文件，用于测试异常!!")
except IOError:
    print "Error: 没有找到文件或读取文件失败"
else:
    print "内容写入文件成功"
    fh.close()
```

在执行代码前为了测试方便，我们可以先去掉 **testfile** 文件的写权限，命令如下： 

```python
chmod -w testfile
```

再执行以上代码： 

```python
$ python test.py 
Error: 没有找到文件或读取文件失败
```

### 使用 except 而不带任何异常类型

你可以不带任何异常类型使用 **except**，如下实例：

```python
try:
    正常的操作
   ......................
except:
    发生异常，执行这块代码
   ......................
else:
    如果没有异常执行这块代码
```

以上方式 **try-except** 语句捕获所有发生的异常。但这不是一个很好的方式，我们不能通过该程序识别出具体的异常信息。因为它捕获所有的异常。 

## 使用 except 而带多种异常类型

你也可以使用相同的 **except** 语句来处理多个异常信息，如下所示： 

```python
try:
    正常的操作
   ......................
except(Exception1[, Exception2[,...ExceptionN]]]):
   发生以上多个异常中的一个，执行这块代码
   ......................
else:
    如果没有异常执行这块代码
```

### try-finally 语句

**try-finally** 语句无论是否发生异常都将执行最后的代码。 

```python
try:
<语句>
finally:
<语句>    #退出try时总会执行
raise
```

当在 **try** 块中抛出一个异常，立即执行 **finally** 块代码。 

### 异常的参数

**1、**一个异常可以带上参数，可作为输出的异常信息参数。

**2、**你可以通过 **except** 语句来捕获异常的参数，如下所示：

```python
try:
    正常的操作
   ......................
except ExceptionType, Argument:
    你可以在这输出 Argument 的值...
```





































