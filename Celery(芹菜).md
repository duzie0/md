# Celery(芹菜)

##  描述:

Celery 是用python实现的一个分布式的任务处理系统。

*Celery* 可以单机运行，也可以在多台机器上运行，甚至可以跨越数据中心运行。

Celery 库在使用之前必须初始化，一个celery实例被称为一个应用（或者缩写 app）。

Celery 应用是线程安全的，所以多个不同配置、不同组件、不同任务的 应用可以在一个进程空间里共存。

## 主要组件：

- broker：消息中间件，用来在客户端和任务执行单元之间传递消息
- worker：任务执行单元，从broker中获取任务消息，并执行异步任务
- backend：存储后端，保存任务中间状态及最终结果

broker 和 worker 必须配置，对于结果和任务当前状态不敏感的应用可不配置backend。

*Celery* 需要一个发送和接受消息的传输者。

RabbitMQ 和 Redis 中间人的消息传输支持所有特性。

## celery的优缺点：

### 优点：

- **简单**

  > Celery 易于使用和维护，并且它 *不需要配置文件* 。
  >
  > Celery 有一个活跃、友好的社区来让你寻求帮助，包括一个 邮件列表 和一个 *IRC 频道*。
  >
  > 下面是一个你可以实现的最简应用：
  >
  > ```
  > from celery import Celery
  >
  > app = Celery('hello', broker='amqp://guest@localhost//')
  >
  > @app.task
  > def hello():
  >     return 'hello world'
  > ```

- **高可用性**

  > 倘若连接丢失或失败，职程和客户端会自动重试，并且一些中间人通过 *主/主*或 *主/从* 方式复制来提高可用性。

- **快速**

  > 单个 Celery 进程每分钟可处理数以百万计的任务，而保持往返延迟在亚毫秒级（使用 RabbitMQ、py-librabbitmq 和优化过的设置）。

- **灵活**

  > *Celery* 几乎所有部分都可以扩展或单独使用。可以自制连接池、 序列化、压缩模式、日志、调度器、消费者、生产者、自动扩展、 中间人传输或更多。

### 缺点：

## Celery的常见场景：

1. Web应用。当用户触发的一个操作需要较长时间才能执行完成时，可以把它作为任务交给Celery去异步执行，执行完再返回给用户。这段时间用户不需要等待，提高了网站的整体吞吐量和响应时间。
2. 定时任务。生产环境经常会跑一些定时任务。假如你有上千台的服务器、上千种任务，定时任务的管理很困难，Celery可以帮助我们快速在不同的机器设定不同种任务。
3. 同步完成的附加工作都可以异步完成。比如发送短信/邮件、推送消息、清理/设置缓存等。
   Celery还提供了如下的特性：
4. 方便地查看定时任务的执行情况，比如执行是否成功、当前状态、执行任务花费的时间等。
5. 可以使用功能齐备的管理后台或者命令行添加、更新、删除任务。
6. 方便把任务和配置管理相关联。
7. 可选多进程、Eventlet和Gevent三种模式并发执行。
8. 提供错误处理机制。
- 提供多种任务原语，方便实现任务分组、拆分和调用链。
- 支持多种消息代理和存储后端。

## Celery模块架构

![图 1. Celery 的模块架构](https://www.ibm.com/developerworks/cn/opensource/os-cn-celery-web-service/img001.png)

**任务生产者 (task producer)**

任务生产者 (task producer) 负责产生计算任务，交给任务队列去处理。在 Celery 里，一段独立的 Python 代码、一段嵌入在 Django Web 服务里的一段请求处理逻辑，只要是调用了 Celery 提供的 API，产生任务并交给任务队列处理的，我们都可以称之为任务生产者。

**任务调度器 (celery beat)**

Celery beat 是一个任务调度器，它以独立进程的形式存在。Celery beat 进程会读取配置文件的内容，周期性地将执行任务的请求发送给任务队列。Celery beat 是 Celery 系统自带的任务生产者。系统管理员可以选择关闭或者开启 Celery beat。同时在一个 Celery 系统中，只能存在一个 Celery beat 调度器。

**任务代理 (broker)**

任务代理方负责接受任务生产者发送过来的任务处理消息，存进队列之后再进行调度，分发给任务消费方 (celery worker)。因为任务处理是基于 message(消息) 的，所以我们一般选择 RabbitMQ、Redis 等消息队列或者数据库作为 Celery 的 message broker。

**任务消费方 (celery worker)**

Celery worker 就是执行任务的一方，它负责接收任务处理中间方发来的任务处理请求，完成这些任务，并且返回任务处理的结果。Celery worker 对应的就是操作系统中的一个进程。Celery 支持分布式部署和横向扩展，我们可以在多个节点增加 Celery worker 的数量来增加系统的高可用性。在分布式系统中，我们也可以在不同节点上分配执行不同任务的 Celery worker 来达到模块化的目的。

**结果保存**

Celery 支持任务处理完后将状态信息和结果的保存，以供查询。Celery 内置支持 rpc, Django ORM，Redis，RabbitMQ 等方式来保存任务处理后的状态信息。

## 使用：

### 创建一个任务：

```python
from celery import Celery
app = Celery('task',broker='redis://localhost:6379/1',
             backend='redis://localhost:6379/2')
@app.task
def fib(n):
    if n in [1,2]:
        return 1
    elif n > 2:
        return fib(n-1) + fib(n-2)
    else:
        raise Exception('input n invalid')
 --------------------------------------------------------------------------
celery -A task -l info worker
-----------------------------------------------------------------------------
client:
from tasks import task
res = task.delay(10)
print(res.state)
print(res.result)
```

任务上可以设置很多选项，这些选项作为参数传递给装饰器：

使用多个装饰器装饰任务函数时，确保 `task` 装饰器最后应用（在python中，这意味它必须在第一个位置）

```python
from models import User
@app.task(serializer='json')
def create_user(username,password):
    User.object.create(username=username,password=password)
```

### 利用调度器创建周期任务：

#### 配置文件

Celery 的调度器的配置是在 CELERYBEAT_SCHEDULE 这个全局变量上配置

> \# config.py
>
> from datatime import timedelta
>
> CELERYBEAT_SCHEDULE = {
>
> ​	'select_populate_book' : {
>
> ​		'task':'favorite_book.select_populate_book',
>
> ​		'schedule': timedelta(seconds=100)
>
> }
>
> }

#### 创建Celery对象：

> \#favourite_book.py
>
> from celery import Celery
>
> import time
>
> app = Celery('select_populate_book',backend='rpc://',borker='amqp://localhost')
>
> app.config_from_object('config')
>
> @app.task
>
> def select_populate_book():
>
> ​	print('............')
>
> ​	return True

#### 启动Celery worker

`celery -A favourite_book worker --loglevel=info`

#### 启动Celery beat

启动 Celery beat 调度器，Celery beat 会周期性地执行在 CELERYBEAT_SCHEDULE 中定义的任务，即周期性地查询当前一小时最热门的书籍。

`celery -A favourite_book beat`

### 绑定任务：

个绑定任务意味着任务函数的第一个参数总是任务实例本身(`self`)，就像 Python 绑定方法类似

绑定任务在这些情况下是必须的：

- 任务重试（使用 `app.Task.retry()` )
- 访问当前任务请求的信息
- 添加到自定义任务基类的附加功能

### 任务继承

任务装饰器的 `base` 参数可以声明任务的基类：

```python
import celery

class MyTask(celery.Task):

    def on_failure(self, exc, task_id, args, kwargs, einfo):
        print('{0!r} failed: {1!r}'.format(task_id, exc))

@task(base=MyTask)
def add(x, y):
    raise KeyError()
```

### 任务名称：

每个任务必须有不同的名称。

如果没有显示提供名称，任务装饰器将会自动产生一个

产生的名称会基于这些信息：

- 任务定义所在的模块
- 任务函数的名称

显示设置任务名称：

```python
>>> import celery
>>> app = celery.Celery()
>>> @app.task(name='task_add')
... def add(x,y):
...     return x+y
... 
>>> add.name
'task_add'
```

最佳实践是使用模块名称作为命名空间，这样的话如果有一个同名任务函数定义在其他模块也不会产生冲突。

### 导入：

```python
>>> from project.myapp.tasks import mytask
>>> mytask.name
'project.myapp.tasks.mytask'

>>> from myapp.tasks import mytask
>>> mytask.name
'myapp.tasks.mytask'
----------------------------------------------------------------
from module import foo   # BAD!

from proj.module import foo  # GOOD!
```

### 改变自动名称生成形式

```python
from celery import Celery
class MyCelery(Celery):
    def gen_task_name(self, name, module):
        if module.endswith('.tasks'):
            module = module[:-6]
        return super(MyCelery, self).gen_task_name(name, module)
```

### 任务请求

`app.Task.request` 包含于当前执行任务相关的信息与状态

```python
@app.task(bind=True)
def dump_context(self, x, y):
    print('Executing task id {0.id}, args: {0.args!r} kwargs: {0.kwargs!r}'.format(
            self.request))
```

### 重试

`app.Task.retry()` 函数可以用来重新执行任务，例如在可恢复错误的事件中。

```python
@app.task(bind=True)
def send_twitter_status(self, oauth, tweet):
    try:
        twitter = Twitter(oauth)
        twitter.update_status(tweet)
    except (Twitter.FailWhaleError, Twitter.LoginError) as exc:
        raise self.retry(exc=exc)
```

`app.Task.retry()` 调用将会抛出一个异常使得任意 `retry` 后面的代码都不会被执行。这个异常就是 `Retry` 异常.



