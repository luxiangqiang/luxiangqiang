---
title:  数据分析之 Python 【NumPy库】
categories:
- python
- 数据分析
tags: 
- python
- 数据分析
---

![](/images/Numpy库的使用.png)

NumPy 是一个重要的 Python 库，有哪些操作呢？

<!--more-->



### 一、Numpy是什么？

> Python 中使用最多的第三方库，NumPy 所提供的数据结构是 Python 数据分析的基础。



###### ▍两个对象

> 1、ndarray解决了多维数组问题。
>
> 2、ufunc 则是解决对数组进行处理的函数。



### 二、Numpy 和 list 的区别？

> 1、 Python中的 list 保存的是对象的指针，因此数据量大时很占内存，所以会慢。 
>
> 2、NumPy 数组存储在一个均匀连续的内存块中，这样数组计算遍历所有的元素，不像列表 list 还需要对内存地址进行查找，从而节省了计算资源，比较快。 



### 三、Numpy 的使用

#### 1、安装 Numpy

> 使用 pip 命令进行安装，如果 pip 不是内外部命令，则在 path 环境变量中进行配置。

```
pip install numpy
```



#### 2、创建数组

> 在 NumPy 数组中，维数称为秩（rank），一维数组的秩为 1，二维数组的秩为 2，以此类推。在 NumPy 中，每一个线性的数组称为一个轴（axes），其实秩就是描述轴的数量。

```python
import numpy as np

a = np.array([1,2,3])
b = np.array([[1,2,3],[4,5,6],[7,8,9]])
//添加数据
b[1,1] = 10
// 获取数组大小
print(a.shape)
print(b.shape)
//获取元素类型
print(a.dtype)
print(b)
```



#### 3、结构数组

> 总体数量中的某一类数据，求平均数等操作。

```python
import numpy as np

persontype = np.dtype({
    'names':['name','age','chinese','math','english'],
    'formats':['S32','i', 'i', 'i', 'f']})

# 添加数据,参数二：设置数组的类型
persons = np.array([("ZhangFei",32,75,100, 90),
                    ("GuanYu",24,85,96,88.5),
                    ("ZhaoYun",28,85,92,96.5),
                    ("HuangZhong",29,65,85,100)],dtype = persontype)

# 统计每个人的年龄
ages = persons[:]['age']
print(ages)

# 计算年龄的平均数
print(np.mean(ages))
```

- **S32** : 32个字节的字符串类型，由于结构中的每个元素的大小必须固定，因此需要指定字符串的长度
- **i** : 32bit的整数类型，相当于np.int32
- **f** : 32bit的单精度浮点数类型，相当于np.float32



#### 4、ufunc 运算

> 对数组中的每个元素进行操作。ufunc 计算速度快，底层是 c 语言实现的。



#### 5、连续数组的创建

> 使用 arange 或 linspace 创建连续函数。



###### ▉ arange

> 通过指定**（初始值、终值、步长）**来创建等差数列的一维数组，默认不包括终值。

```python
x1 = np.arange(1,11,2)//[1 3 5 7 9]
```



###### ▉ linspace

> 通过指定**（初始值、终值、元素个数）**来创建等差数列的一维数组，默认包括终值。



#### 6、算数运算

> 通过 NumPy 可以自由地创建等差数组，同时也可以进行加、减、乘、除、求 n 次方和取余数。

```python
import numpy as np
x1 = np.arange(1,11,2)
x2 = np.linspace(1,9,5)

# 相加
print(np.add(x1,x2))

# 相减
print(np.subtract(x1,x2))

# 相乘
print(np.multiply(x1,x2))

# 相除
print(np.divide(x1,x2))

# 求 n 次方
print(np.power(x1,x2))

# 取余
print(np.remainder(x1,x2))
print(np.mod(x1,x2))
```



#### 7、统计函数

> 对大量数据进行描述性的统计分析。最大值、最小值、平均值，是否符合正态分布、方差、标准差多少等等。



###### ▉ 计算数组/矩阵的最大值函数(amax()),最小值函数 (amin())

```python
import numpy as np

a = np.array([[1,2,3],[4,5,6],[7,8,9]])

# 数组中的最大值
print(np.amax(a))

# 延 axis=0 轴(纵轴)的最小值
print(np.amin(a,0))

# 延 axis=1 轴(横轴)的最小值
print(np.amin(a,1))
```



###### ▉ 统计最大值与最小值之差

```pyhon
# 数组中最大值和最小值之差
print(np.ptp(a))

# 延 axis=0 轴(纵轴)最大值和最小值之差
print(np.ptp(a,0))

# 延 axis=1 轴(横轴)最大值和最小值之差
print(np.ptp(a,1))
```



###### ▉ 统计数组的百分位数 percentile()

> percentile() 代表着第 p 个百分位数，这里 p的取值范围是 0-100，如果 p=0，那么就是求最小值，如果 p=50 就是求平均值，如果 p=100 就是求最大值。同样你也可以求得在 axis=0 和 axis=1 两个轴上的 p% 的百分位数。

```python
a = np.array([[1,2,3],[4,5,6],[7,8,9]])

# 统计数组的百分位数
print(np.percentile(a,100))//9.0

print np.percentile(a, 50, axis=0)
print np.percentile(a, 50, axis=1)
```



###### ▉ 统计数组中的中位数 median、平均数 mean()

```python
# 求中位数
print(np.median(a))

# 求中位数
print(np.mean(a))
```



###### ▉ 统计数组中的加权平均值 average()

> average() 函数可以求加权平均，加权平均的意思就是每个元素可以设置个权重，默认情况下每个元素的权重是相同的，所以np.average(a)=(1+2+3+4)/4=2.5，你也可以指定权重数组 wts=[1,2,3,4]，这样加权平平均 np.average(a,weights=wts)=(1*1+2*2+3*3+4*4)/(1+2+3+4)=3.0。

```python
# 统计数组的加权平均值
a = np.array([1,2,3,4])
wts = np.array([1,2,3,4])
print(np.average(a))
print(np.average(a,weights=wts))
```



###### ▉ 统计数组中的标准差 std()、方差 var()

> 方差的计算是指每个数值与平均值之差的平方求和的平均值,标准差是方差的算术平方根(**代表的是一组数据离平均值的分散程度**)。

```python
# 求标准差和方差
a = np.array([1,2,3,4])
print(np.std(a))
print(np.var(a))
```



#### 8、NumPy 排序

```python
sort(a, axis=-1, kind=‘quicksort’, order=None)
```

> 1、使用 sort 函数，kind 的值可以指定为 quicksort 、mergesort、heapsort分别表示快速排序、归并排序、堆排序，一般使用快速排序。
>
> 2、同样 axis 默认是 -1，即沿着数组的最后一个轴进行排序，也可以取不同的 axis 轴，或者 axis=None 代表采用扁平化的方式作为一个向量进行排序。
>
> 3、order 字段，对于结构化的数组可以指定按照某个字段进行排序。

```python
# NumPy 排序
# 4 3 2
# 2 4 1
a = np.array([[4,3,2],[2,4,1]])
# 横轴排序
print(np.sort(a,axis=-1))
# 纵轴排序
print(np.sort(a,axis=0))
print(np.sort(a,axis=1))

# 结果
[[2 3 4]
 [1 2 4]]
---------
[[2 3 1]
 [4 4 2]]
---------
[[2 3 4]
 [1 2 4]]
```













