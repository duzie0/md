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

### 数据类型转换：

| 输入                       | 输出                |
| ------------------------ | ----------------- |
| >>>int('123')            | 123               |
| >>>float('12.34')        | 12.34             |
| >>>str(123)              | '123'             |
| >>>bool(1)               | True              |
| >>>bool(None)            | False             |
| >>>bool('')              | False             |
| >>>bool(' ')             | True              |
| >>>bool()                | False             |
| >>>list('abc')           | ['a', 'b', 'c']   |
| >>>list((1,2,3))         | [1, 2, 3]         |
| >>> str(['a', 'b', 'c']) | "['a', 'b', 'c']" |
| >>>tuple([1,2,3])        | (1,2,3)           |
| >>>set([1,2,3])          | {1,2,3}           |
|                          |                   |

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

**dict的key必须是不可变对象，且唯一。**

在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。而list是可变的，就不能作为key

dict可以根据key 来修改value，**value可以相同**。

**dict查询速度快的原因：**

创建字典是根据key进行哈希运算得到value的存储位置。

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

pop()随机移除一个值并返回：

```python
>>> s = {1,2,3}
>>> s.pop()
1
>>> s
{2, 3}
```

当集合为空时：

```python
>>>s
set()
>>> s.pop()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'pop from an empty set'
```

清空集合：`s.clear()`

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

## Python函数：

### 函数返回：

1. 函数里**没有return默认返回None**
2. 函数在语法上，**返回一个tuple可以省略括号**，而多个变量可以同时接收一个tuple，按位置赋给对应的值，所以，Python的函数返回多值其实就是返回一个tuple，但写起来更方便。

### 函数参数：

#### 位置参数：positional argument

```python
def add(x,y,z):
    return 100*x + 10*y + z
add(1,2,3)
```

当调用函数是所有的位置参数都得传入

#### 默认参数：default argument

默认参数可以简化函数的调用，降低调用函数的难度。

**设置默认参数时，有几点要注意：**

1. 必选参数在前，默认参数在后，否则Python的解释器会报错
2. 把变化大的参数放前面，变化小的参数放后面。变化小的参数就可以作为默认参数。

**默认参数最大的坑：**

```python
>>> def add(l=[]):
...     l.append('我是小尾巴')
...     return l
...
>>> add([1,2,3])
[1, 2, 3, '我是小尾巴']
>>> add()
['我是小尾巴']
>>> add()
['我是小尾巴', '我是小尾巴']
```

Python函数在定义的时候，默认参数`L`的值就被计算出来了，即`[]`，因为默认参数`L`也是一个变量，它指向对象`[]`，每次调用该函数，如果改变了`L`的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的`[]`了。

**默认参数必须指向不变对象！**

**优化：**

```python
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L
```

#### 可变参数:

可变参数就是传入的参数个数是可变的。

在参数前面加了一个`*`号。

**一种常见的写法：**

```python
def test(*num):
    pass
L = [1,2,3]
-----------------------------------------------
test(L[0],L[1],L[2])
test(*L)
#*L表示把L这个list的所有元素作为可变参数传进去
-----------------------------------------------
```

#### 关键字参数:

在参数前面加了两个`*`号。

```python
def info(name,sex,**kw):
    pass
D = dict(city='beijing',job='worker',hobby='music')
----------------------------------------------------
info('zhang','男'，**D)
info('li','男'，city=D['city'],job=D['job'],hobby=D['hobby'])
----------------------------------------------------
```

`**D`表示把`D`这个dict的所有key-value用关键字参数传入到函数的`**kw`参数，`kw`将获得一个dict，注意`kw`获得的dict是`D`的一份拷贝，对`kw`的改动不会影响到函数外的`D`。

#### 命名关键字：

要限制关键字参数的名字，就可以用命名关键字参数

例如，限制上面的关键字参数，只接收city和job：

```python
def info(name,sex,*,city,job):
    pass
--------------------------------------------------
info('zhao','男',city='shanghai',job='docter')
```

命名关键字参数需要一个特殊分隔符`*`，`*`后面的参数被视为命名关键字参数

如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了：

```python
def info(name,*args,city,job):
    pass
-------------------------------
info('wang',city='nanjing',job='teacher')
```

**命名关键字都要传参。**

命名关键字也可以使用默认值。

#### 组合关键字：

以上5种关键字可以组合使用，但是要注意定义的顺序：

位置(必选)参数-->默认参数-->可变参数-->命名关键字参数-->关键字参数

```python
def f1(a,b,c=0,*args,**kwargs):
    print('a =',a,' b =',b,' c =',c,' args =',args,' kwargs =',kwargs)
def f2(a,b,c=0,*,city,**kwargs):
    print('a =',a,' b =',b,' c =',c,' city =',city,' kwargs =',kwargs)
--------------------------------------------------------------------------------------------
>>> f1(1,2)
a = 1  b = 2  c = 0  args = ()  kwargs = {}
>>> f2(1,2,3,city='nanjing')
a = 1  b = 2  c = 3  city = nanjing  kwargs = {}
>>> f2(1,2,3,city='nanjing',bb='aaaaa')
a = 1  b = 2  c = 3  city = nanjing  kwargs = {'bb': 'aaaaa'}
-----------------------------------------------------------------------------------------------
>>> L = [1,2,3,4,5,6]
>>> D = dict(city='nanjing',job='waiter',hobby='music',age=20)
>>> f1(*L,**D)
a =  1  b =  2  c =  3  args =  (4, 5, 6)  kwargs =  {'city': 'nanjing', 'job': 'waiter', 'hobby': 'music', 'age': 20}
--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--	--
>>> L = [1,2,3]
>>> f2(*L,**D)
a = 1  b = 2  c = 3  city = nanjing  kwargs = {'job': 'waiter', 'hobby': 'music', 'age': 20}
```

**不要同时使用太多的组合，否则函数接口的可理解性很差。**

## Python 内置函数

**编码：**

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

**len()函数：**

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

**input()函数：**

input()函数会阻塞程序。

input()返回的数据类型是str。

**range()函数：**

`range(5)` 生成整数数列0,1,2,3,4，不包括5。

`range(1,5)`1,2,3,4

`range(0,5,2)`0,2,4，第三个参数是步长

**isinstance():** 

```python
>>> isinstance('aaa',(int,str))
True
>>> isinstance(123,int)
True
>>> isinstance(123,str)
False
```

**enumerate():**

```python
>>> L
[1, 2, 3, 4, 5]
>>> for index,value in enumerate(L):
...     print(index,value)
...
0 1
1 2
2 3
3 4
4 5
```

## Python高级功能

### 切片：

首先，切片不是list所特有的，tuple,string也有这种功能。

```python
>>> L = [1,2,3,4,5]
>>> L[:]		#原列表
[1, 2, 3, 4, 5]
>>> L[::]		#原列表
[1, 2, 3, 4, 5]
>>> L[:4:2]
[1, 3]
>>> L[::2]
[1, 3, 5]
>>> L[::-1]		#-1表示截取的步长
[5, 4, 3, 2, 1]
>>> L[4:1:-1]	#步长为负时起至的索引要从后往前，否则切片的是空列表
[5, 4, 3]
>>> L[1:4:-1]
[]
```

对于元组切片的结果还是元组，字符串切片结果还是字符串。

```python
>>> t = (1,2,3,4,5)
>>> s = 'abcdefghijkl'
>>> t[::-1]
(5, 4, 3, 2, 1)
>>> t[:2]
(1, 2)
>>> s[::-1]
'lkjihgfedcba'
>>> s[1:4] 		#不包括索引为4的值
'bcd'
```

### 迭代：

如果给定一个list或tuple，通过`for`循环来遍历这个list或tuple，这种遍历称为迭代（Iteration）。

```python
>>> from collections import Iterable
>>> isinstance(123,Iterable)
False
>>> isinstance('abc',Iterable)
True
```

迭代必须是可迭代对象。

### 列表生成式：

```python
>>> L = [i for i in range(10)]
>>> L
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> type(L)
<class 'list'>
```

### 生成器：

Python中，一边循环一边计算的机制，称为生成器：generator。

**创建方式：**

将列表生成式的`[]` 换成`()` 的到的即是一个生成器。

```python
>>> g = (i for i in range(10))
>>> g
<generator object <genexpr> at 0x000002F4CD147E08>
>>> next(g)
0
>>> next(g)
1
.......
>>> next(g)
9
>>> next(g)			#当迭代器中没有元素会抛出StopIteration
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
---------------------------------------------------------------------
>>> g = (i for i in range(10))
>>> for i in g:
...     print(i)
...
0
1
2
3
4
5
6
7
8
9
```

**使用yield关键字创建：**

```python
def fib(max):
    n,a,b = 0,0,1
    while n < max:
        yield b
        a,b = b,a+b
        n += 1
     return 'done'
f = fib(5)
for i in f:
    print(i)
--------------------------------------------------------
f = fib(5)
while True:
    try:
        print(next(f))
     except StopItertion as e:
        print('返回值',e.value)
        break
```

输出：

```python
1
1
2
3
5
1
1
2
3
5
返回值 done
```

### 迭代器：

可以直接作用于`for`循环的数据类型有以下几种：

一类是集合数据类型，如`list`、`tuple`、`dict`、`set`、`str`等；

一类是`generator`，包括生成器和带`yield`的generator function。

这些可以直接作用于`for`循环的对象统称为可迭代对象：`Iterable`。

可以被`next()`函数调用并不断返回下一个值的对象称为迭器：`Iterator`。

使用`isinstance()`判断一个对象是否是`Iterable`对象。

**小结：**

1. 凡是可作用于for循环的对象都是Iterable类型；
2. 凡是可作用于next()函数的对象都是Iterator类型，它们表示一个惰性计算的序列；
3. 集合数据类型如list、dict、str等是Iterable但不是Iterator，不过**可以通过iter()函数获得一个Iterator对象**。
4. Python的for循环本质上就是通过不断调用next()函数实现的

## 异常：

## Python2.x与Python3.x

| 不同点  | Python2.x | Python3.x |
| ---- | --------- | --------- |
|      |           |           |
|      |           |           |
|      |           |           |



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

类比于数学中的一些公式：当思考一道题目的解决办法时，更多的是先用各种公式来组成解题的逻辑过程,

### 接口：

**函数的接口：** 很显然，当函数的参数确定下来，函数的接口就定义完成了，只需要知道要传递什么样的参数以及函数的返回值是什么，忽略函数内部复杂的逻辑。

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