# Python内置库和模块

## logging

### 组成：

#### **日志记录器：** Logger

日志对象-logging.getLogger()-默认的root记录器

记录器负责管理日志消息的默认行为，包括日志记录级别、输出目标位置、消息格式以及其它基本细节。

如下是处理器Handler关键的参数：

| 关键字参数 | 描述                                       |
| ---------- | ------------------------------------------ |
| filename   | 将日志消息附加到指定文件名的文件           |
| filemode   | 指定用于打开文件模式                       |
| format     | 用于生成日志消息的格式字符串               |
| datefmt    | 用于输出日期和时间的格式字符串             |
| level      | 设置记录器的级别                           |
| stream     | 提供打开的文件，用于把日志消息发送到文件。 |

#### 格式化器：Formater

 '%(asctime)s  %(module)s  %(level)s  %(lineno)d'

| 格式           | 描述                               |
| -------------- | ---------------------------------- |
| %(name)s       | 记录器的名称, 默认为root           |
| %(levelno)s    | 数字形式的日志记录级别             |
| %(levelname)s  | 日志记录级别的文本名称，如`error`  |
| %(filename)s   | 执行日志记录调用的源文件的文件名称 |
| %(pathname)s   | 执行日志记录调用的源文件的路径名称 |
| %(funcName)s   | 执行日志记录调用的函数名称         |
| %(module)s     | 执行日志记录调用的模块名称         |
| %(lineno)s     | 执行日志记录调用的行号             |
| %(created)s    | 执行日志记录的时间                 |
| %(asctime)s    | 日期和时间                         |
| %(msecs)s      | 毫秒部分                           |
| %(thread)d     | 线程ID                             |
| %(threadName)s | 线程名称                           |
| %(process)d    | 进程ID                             |
| %(message)s    | 记录的消息                         |

#### 处理器：Handler

 logging模块提供了一些处理器，可以通过各种方式处理日志消息。使用addHandler()方法将这些处理器添加给Logger对象。另外还可以为每个处理器配置它自己的筛选和级别。

​      handlers.DatagramHandler(host，port):发送日志消息给位于制定host和port上的UDP服务器。

​      * handlers.FileHandler(filename): 将日志消息写入文件filename。

​      handlers.HTTPHandler(host, url):使用HTTP的GET或POST方法将日志消息上传到一台HTTP 服务器。

​      * handlers.RotatingFileHandler(filename):将日志消息写入文件filename。如果文件的大小超出maxBytes制定的值，那么它将被备份为filename1。

​    由于内置处理器还有很多，如果想更深入了解。可以查看官方手册。

#### 过滤器：Filter

### 日志记录级别

  logging模块的重点在于生成和处理日志消息。每条消息由一些文本和指示其严重性的相关级别组成。级别包含符号名称和数字值。

| 级别           | 值   | 描述          |
| -------------- | ---- | ------------- |
| CRITICAL/FATAL | 50   | 关键错误/消息 |
| ERROR          | 40   | 错误          |
| WARNING        | 30   | 警告消息      |
| INFO           | 20   | 通知消息      |
| DEBUG          | 10   | 调试          |
| NOTSET         | 0    | 无级别        |

如果设置日志级别为`警告`则日志记录器不会输出`警告`以下的日志信息，会输出包括`警告` 及以上的日志信息。

### 简单地用法

```python
# 设置日志等级
logging.getLogger().setLevel(logging.INFO)
formatter = '%(asctime)s: %(filename)s/%(funcName)s at %(lineno)s->%(message)s'

# 配置日志的信息，filename 要指定日志输出的文件名
logging.basicConfig(format=formatter,
                    datefmt='%Y-%m-%d %H:%M:%S',
                    filename='art.log',
                    filemode='a')

logging.warning('--当前页面要被缓存5秒---')
```



## os

```python
os.name
os.uname()
os.environ
os.environ.get('key')
os.getcwd()
os.mkdir('path')
os.rmdir('path')
os.rename('path','path') #可以是目录也可以是文件
os.remove('filename')
os.listdir('path')
os.path.isdir('path')
os.path.isfile('path')
os.path.abspath('path')
os.path.join('path','path')
os.path.split('path')
os.path.splittext('path')
os.path.exists('path')
```

`shutil`模块提供了`copyfile()`的函数，可以看做是`os`模块的补充。

```python
os.fork()#linux下创建子进程
os.kill()
os.system(command='')#命令行
os.getpid()
```



## re

match

## random

```python
import random
l = [324,53,3233,4,332,45]
m = l[:3]
print(m)
s = l.index(4)
print(s)
a = random.random()
print('0-1之间的随机小数:%.2f'%a)
b = random.randrange(2,5)
print('2-5之间的随机整数:',b)
c = random.uniform(1,9)
print('1-9之间的随机小数:%.3f'%c)
d = random.randint(1,10)
print('1-10随机整数:',d)
e = random.choice(l)
print('从l中随机选一个数:',e)
```

输出：

```python
[324, 53, 3233]
3
0-1之间的随机小数:0.23
2-5之间的随机整数: 4
1-9之间的随机小数:6.721
1-10随机整数: 4
从l中随机选一个数: 53
```

## time&datetime

```python
import time
t1 = time.time()
t2 = time.gmtime()
t3 = time.localtime()
print(t1,'\n',t2,'\n',t3)
t4 = time.strftime('%Y{}%m{}%d{} %H{}%M{}%S{}',t3).format('年','月','日','时','分','秒')
time.sleep(1)
t = (2009, 2, 17, 17, 3, 38, 1, 48, 0)
t5 = time.mktime(t)
t6 = time.strptime('2018年04月08日 11时01分38秒','%Y{}%m{}%d{} %H{}%M{}%S{}'.format('年','月','日','时','分','秒'))
print(t4,'\n',t5,'\n',t6)
```

输出：

```python
1534815847.8309767 
time.struct_time(tm_year=2018, tm_mon=8, tm_mday=21, tm_hour=1, tm_min=44, tm_sec=7, tm_wday=1, tm_yday=233, tm_isdst=0) 
time.struct_time(tm_year=2018, tm_mon=8, tm_mday=21, tm_hour=9, tm_min=44, tm_sec=7, tm_wday=1, tm_yday=233, tm_isdst=0)
2018年08月21日 09时44分07秒 
1234861418.0 
time.struct_time(tm_year=2018, tm_mon=4, tm_mday=8, tm_hour=11, tm_min=1, tm_sec=38, tm_wday=6, tm_yday=98, tm_isdst=-1)
```



## functools

### hashlib

```python
import hashlib
md5 = hashlib.md5()
md5.update('how to use md5 in python hashlib?'.encode('utf-8'))
print(md5.hexdigest())
```

输出：

```python
d26a53750bc40b38b65a520292f69306
```



