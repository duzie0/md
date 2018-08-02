# Python基础回顾

## Python数据类型：

#### Number

整形，浮点数

#### 小整数对象——小整型对象池

在实际编程中，数值比较小的整数，比如1,2,29等，可能会非常频繁的出现。而在python中，所有的对象都存在于系统堆上。如果某个小整数出现的次数非常多，那么Python将会出现大量的malloc/free操作，这样大大降低了运行效率，而且会造成大量的内存碎片，严重影响Python的整体性能。

在Python2.5以后，将位于[-5,257)之间的小整数，缓存在小整型对象池中。

```python
a = 1
b = 1
c = 23436456
print(id(a),id(b),id(c))
------------------------------------------------------------------------------------
output:
10943008 10943008 140356407334832
b不再开辟新的内存空间
```

**Boolean：**True，False

**String：**字符串

**List：** 列表

**Tuple：** 元组

**Dict：** 字典

**Set：** 集合

**None：** 空值

## String操作：



## List,Tuple,Dict,Set

### List:列表

list是一个可变的有序表

使用列表的索引注意索引不要越界，否则报IndexError错误。

#### 对list进行一些操作：

往list中追加元素到末尾：

```python
>>> classmates = ['Michael', 'Bob', 'Tracy']
>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']
```

把元素插入到指定的位置，比如索引号为`1`的位置：

```python
>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
```

删除list末尾的元素，用`pop()`方法：

```python
>>> classmates.pop()
'Adam'
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy']
```

删除指定位置的元素，用`pop(i)`方法，其中`i`是索引位置：

```python
>>> classmates.pop(1)
'Jack'
>>> classmates
['Michael', 'Bob', 'Tracy']
```

把某个元素替换成别的元素，可以直接赋值给对应的索引位置：

```python
>>> classmates[1] = 'Sarah'
>>> classmates
['Michael', 'Sarah', 'Tracy']
```

### Tuple:元组

tuple一旦初始化就不能修改,，是不可变的有序表

没有append()，insert()这样的方法，获取元素的方法和list是一样的，但不能赋值

**定义元组：**

```python
>>>t = ()
>>>t
()			#t是一个空元组
-------------------------------------------------------------------
>>>t = (1)
>>>t
1			#此时解释器把t当做一个整数，括号是小括号的意思，不是定义元组
----------------------------------------------------------------------
>>>t = (1,)
>>>t
(1,)
```



### Dict:字典

Python内置了字典：dict的支持，dict全称dictionary，在其他语言中也称为map，使用键-值（key-value）存储，具有极快的查找速度。

**dict的key必须是不可变对象。**

在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。而list是可变的，就不能作为key

dict可以根据key 来修改value。

**创建字典：**

```python
>>> dict1 = {'user1':20,'user2':40}
>>> dict1
{'user1': 20, 'user2': 40}
>>> dict2 = dict(user3=58,user4=76)
>>> dict2
{'user3': 58, 'user4': 76}
```

**key-value存储方式：**

```python
>>> dict1['user1']
20
>>> dict1['user1'] = 100
>>> dict1['user1']
100
>>> dict1['user3']
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'user3'
```

如何避免这种错误：

`<key> in <dict>`

```python
>>> 'user3' in dict1
False
>>> 'user3' in dict2
True
>>> dict2['user3']
58
```

要删除一个key，用`pop(key)`方法，对应的value也会从dict中删除：

`dict1.pop('uer1')`



### Set:集合

set是无序的。

不可以放入可变对象。

set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。

创建set:

```python
>>> s = set([1, 1, 2, 2, 3, 3])
>>> s
{1, 2, 3}
#去重，无序
```

通过`add(key)`方法可以添加元素到set中，可以重复添加，但不会有效果：

```python
>>> s.add(4)
>>> s
{1, 2, 3, 4}
>>> s.add(4)
>>> s
{1, 2, 3, 4}
```

通过`remove(key)`方法可以删除元素：

```python
>>> s.remove(4)
>>> s
{1, 2, 3}
```

set可以做数学意义上的交集、并集等操作：

```python
>>> s1 = set([1, 2, 3])
>>> s2 = set([2, 3, 4])
>>> s1 & s2
{2, 3}
>>> s1 | s2
{1, 2, 3, 4}
```

### 不可变对象

一段代码：

```python
>>> a = 'abc'
>>> b = a.replace('a', 'A')
>>> b
#replace方法创建了一个新的字符串对象'Abc'，变量b指向该对象
'Abc'
>>> a
'abc'
```

`a`是变量，而`'abc'`才是字符串对象！有些时候，我们经常说，对象`a`的内容是`'abc'`，但其实是指，`a`本身是一个变量，它指向的对象的内容才是`'abc'`

字符串，数字对象是不可变的

**将(1,2,3)和(1,2,3,[1,2,3])作为dict和set的key：**

```python
>>> t = (1,2,3)
>>> dict1[t] = 565
>>> dict1
{'user1': 20, 'user2': 100, (1, 2, 3): 565}
---------------------------------------------------------
>>> t0 = (1,2,3,[1,2,3])
>>> t0
(1, 2, 3, [1, 2, 3])
>>> dict1[t0] = 7758
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
-----------------------------------------------------------
>>> s = set([1,2,3])
>>> s
{1, 2, 3}
>>> s.add(t)
>>> s
{1, 2, 3, (1, 2, 3)}
>>> s.add(t0)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```



### List与Dict：

和list比较，dict有以下几个特点：

1. 查找和插入的速度极快，不会随着key的增加而变慢；
2. 需要占用大量的内存，内存浪费多。

而list相反：

1. 查找和插入的时间随着元素的增加而增加；
2. 占用空间小，浪费内存很少。

dict是用空间来换取时间的一种方法。

## Python 内置函数

### 编码：

Python的字符串类型是`str`，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把`str`变为以字节为单位的`bytes`。

Python对`bytes`类型的数据用带`b`前缀的单引号或双引号表示。

`'ABC'.encode()` **编码，把字符串编码为bytes**

```python
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
>>> '中文'.encode('ascii')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
```

`b'ABC'.decode()` **解码，把bytes变成字符串**

如果`bytes`中包含无法解码的字节，`decode()`方法会报错

传入`errors='ignore'` 关键字参数来实现忽略错误的字节

```python
>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
```

**`ord()` 获取字符的整数表示**

**`chr()` 编码转换为对应的字符**

```python
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'
>>>x = b'ABC'
```

### len()函数：

计算str中包含多少字符：

```python
>>> len('ABC')
3
>>> len('中文')
2
```

如果len传的参数是bytes，则计算的是字节数：

```python
>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('utf-8'))
6
```

如果传的是list,dict,tuple,set，计算的是他们的长度；

```python
>>> l = [1,2,3,4]
>>> len(l)
4
>>> t = (1,2,3,4)
>>> len(t)
4
>>> d = dict(a=1,b=2)
>>> len(d)
2
>>>s = set(l)
>>>len(s)
4
```

### input()函数：

input()函数会阻塞程序。

input()返回的数据类型是str。

### range()函数：

`range(5)` 生成整数数列0,1,2,3,4，不包括5。



## 重点掌握：

### 编码：

#### ASCII编码：

最早只有127个字符被编码到计算机里，也就是大小写英文字母、数字和一些符号，这个编码表被称为`ASCII`编码，比如大写字母`A`的编码是`65`，小写字母`z`的编码是`122`。

要处理中文显然一个字节是不够的，至少需要两个字节，而且还不能和ASCII编码冲突，所以，中国制定了`GB2312`编码，用来把中文编进去。

各国有各国的标准，就会不可避免地出现冲突，结果就是，在多语言混合的文本中，显示出来会有乱码。

#### Unicode标准：

Unicode把所有语言都统一到一套编码里，解决乱码问题。

Unicode标准最常用的是用两个字节表示一个字符（如果要用到非常偏僻的字符，就需要4个字节）。现代操作系统和大多数编程语言都支持Unicode。

**计算机内存中，统一使用Unicode编码。**

**ASCII编码和Unicode编码的区别：**ASCII编码是1个字节，而Unicode编码通常是2个字节。

字母`A`用ASCII编码是十进制的`65`，二进制的`01000001`；

字符`0`用ASCII编码是十进制的`48`，二进制的`00110000`，注意字符`'0'`和整数`0`是不同的；

汉字`中`超出ASCII编码范围，用Unicode编码是十进制的`20013`，二进制的`01001110 00101101`。

把ASCII编码的`A`用Unicode编码，只需要在前面补0，`A`的Unicode编码是`00000000 01000001`。

### UTF-8编码：可变长编码

UTF-8编码把一个Unicode字符根据不同的数字大小编码成1-6个字节，常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节。如果你要传输的文本包含大量英文字符，用UTF-8编码就能节省空间。

| 字符   | ASCII          | Unicode           | UTF-8                      |
| ---- | -------------- | ----------------- | -------------------------- |
| A    | 01000001       | 00000000 01000001 | 01000001                   |
| 中    | -------------- | 01001110 00101101 | 11100100 10111000 10101101 |

## 一些小的知识点：

### 知识盲点：

一个小的知识盲点可能对一定范围内的知识体系影响不大，不影响开发生产，但盲点的存在就像在脖子上挂了一条绳子，一根无关痛痒，当这种绳子的数目累积到一定的程度，它就扼住了咽喉。

心理上和知识上毫无束缚和阻绊，是突破自身极限应该有的一种状态。

### 函数：

函数是对一个问题的解决办法的一种抽象。

以助于在上层来分析解决其他问题，在解决其它问题的时候暂时不用考虑函数的细节。

类比于数学中的一些公式：当思考一道题目的解决办法时，更多的是先用各种公式来组成解题的逻辑过程，

### 编写代码规范：

- Python的语法比较简单，采用缩进方式。应该始终坚持使用4个空格的缩进
- Python程序是大小写敏感
- Python中，通常用全部大写的变量名表示常量（一种习惯）
- `/` 除法计算结果是浮点数，即使是两个整数恰好整除，结果也是浮点数
- `//` 称为地板除，两个整数的除法仍然是整数

### 注释：

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```

第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释；

第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。

### 占位符：

| 占位符  | **替换内容** |
| ---- | -------- |
| %d   | 整数       |
| %f   | 浮点数      |
| %s   | 字符串      |
| %x   | 十六进制整数   |
| %%   | %        |

也可以使用format()，函数来格式化字符串的输出。