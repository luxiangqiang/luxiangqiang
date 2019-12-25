---
title:  数据分析之 Python 【Pandas】
categories:
- python
- 数据分析
tags:
- python
- 数据分析
---

![](/images/Pandas.png)

如果日常数据清理工作不是非常复杂，几句 Pandas 代码就给你搞定！

<!--more-->



### 一、Series 和 DataFrame

> 分别代表着一维的序列和二维的表结构。基于这两种数据结构，Pandas 可以对数据进行导入、清洗、处理、统计和输出。



#### 1、Series 

> Series 是个定长的字典序列。
>
> 1、Series 有两个属性：values 和 index（默认的index为1,2...），指定 index 比如 index=['a','b','c','d']。

```python
import pandas as pd
from pandas import Series,DataFrame

x1 = Series([1,2,3,4])
x2 = Series(data=[1,2,3,4], index=['a','b','c','d'])
print(x1)
print(x2)

# 输出结果
0    1
1    2
2    3
3    4
dtype: int64
a    1
b    2
c    3
d    4
dtype: int64
```



###### ▉ 数据字典创建

> 用数据字典的方式创建 Series。

```python
# 数据字典创建 Series
d = {'a':1,'b':2,'c':3,'d':4}
x3 = Series(d)
print(x3)
# 结果
a    1
b    2
c    3
d    4
dtype: int64
```



#### 2、DataFrame

> 类似数据库的表。包括行索引和列索引。

```python
import pandas as pd
from pandas import Series,DataFrame
# 数据
data = {
        'Chinese':[66, 95, 93, 90, 80],
        'English':[65, 85, 92, 88, 90],
        'Math':[30, 98, 96, 77, 90]
        }
# 索引默认 1,2,3...
df1 = DataFrame(data)
# 设置索引
df2 = DataFrame(
                data,
    			# 行索引
                index=['ZhangFei', 'GuanYu', 'ZhaoYun', 'HuangZhong', 'DianWei'],
    			# 列索引
                columns=['English', 'Math', 'Chinese']
                )
print(df1)
print(df2)
# 结果
4       80       90    90
            Chinese  English  Math
ZhangFei         66       65    30
GuanYu           95       85    98
ZhaoYun          93       92    96
HuangZhong       90       88    77
DianWei          80       90    90
```



### 二、数据的导入和输出

> Pandas 允许直接从 xlsx,csv 等文件导入数据，也可以输出到这些文件。

```python
# python3.0 安装 openpyxl 包

```





### 三、数据清洗

#### 1、删除 DataFrame 中不必要的行或列

```python
import pandas as pd
from pandas import Series,DataFrame

data = {
    'Chinese': [66, 95, 93, 90,80],
    'English': [65, 85, 92, 88, 90],
    'Math': [30, 98, 96, 77, 90]
}
df2 = DataFrame(data,
                index=['ZhangFei', 'GuanYu', 'ZhaoYun', 'HuangZhong', 'DianWei'],
                columns=['English', 'Math', 'Chinese']
                )
print(df2)
# drop 删除 DataFrame 中不必要的列或行
# columns 删除行
df2 = df2.drop(columns=['Chinese'])
print(df2)

# index 删除列
df2 = df2.drop(index=['ZhangFei'])
print(df2)
```



#### 2、重命命列名 columns，让列表名更容易识别

> 对列名重新命名，直接使用 rename(columns = new_name,inplace=True)函数。

```python
# 重命名列名
df2.rename(columns={'Chinese':'YuWen','English':'YingYu'},inplace = True)
print(df2)
```



#### 3、去重复的值

> 数据采集有重复的行，使用 drop_duplicates() 就会自动把重复的**行**去掉。

```python
# 去除重复行
df = df.drop_duplicates() 
```



#### 4、格式问题

###### ▉ 更改数据格式

> 使用 astype 函数来规范数据格式，比如 Chinese 字段的值改为 str 类型，或者 int64 可以这么写。

```python
# 更改数据格式
df2['YuWen'].astype('str')
df2['YuWen'].astype(np.int64)
```



###### ▉ 数据间的空格

> 用 strip 函数删除数据之间的空格。

```python
# 删除左右两边空格
df2['Chinese']=df2['Chinese'].map(str.strip)
# 删除左边空格
df2['Chinese']=df2['Chinese'].map(str.lstrip)
# 删除右边空格
df2['Chinese']=df2['Chinese'].map(str.rstrip)
```

> 删除某一个字段里的特殊符号。

```python
# 删除某一字段中的特殊符号
df2['YuWen']=df2['YuWen'].str.strip('$')
```



###### ▉ 大小写转换

> 对人名、城市名为了统一都可能用到大小写转换，直接用 upper()、lower()、title() 函数，方法如下。

```python
# 全部大写
df2.columns = df2.columns.str.upper()
# 全部小写
df2.columns = df2.columns.str.lower()
# 首字母大写
df2.columns = df2.columns.str.title()
```



###### ▉ 查找空值

> 数据量大的情况下，有很多字段存在 NaN 的可能，使用 isnull 函数进行查找。

```python
# 查找空值
print(df2.isnull());
# 只显示空值
df2.isnull().any()

# 结果1
            Yingyu   Math  Yuwen
GuanYu       False  False  False
ZhaoYun      False  False   True
HuangZhong   False  False   True
DianWei      False  False   True
# 结果2
Yingyu    False
Math      False
Yuwen      True
dtype: bool
```























































