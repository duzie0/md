# asyncio

##  基本信息：

`asyncio`是Python 3.4版本引入的标准库，直接内置了对异步IO的支持。

`asyncio`的编程模型就是一个消息循环。

从`asyncio`模块中直接获取一个`EventLoop`的引用，然后把需要执行的协程扔到`EventLoop`中执行，就实现了异步IO。

## 一些区别：

@asyncio.coroutine ------------->async

yield from --------------------------->await

## asyncio关键词

### yield from:

yield from iterable = for item in iterable:yield from

yield from 后面必须跟**iterable对象**，一般都是耗时的操作

### **async **:

**async无法将一个生成器转化为协程。**

async定义一个协程，协程是一种对象，需要加入**事件循环loop**才能运行。

async标记的协程调用**不会立刻执行**，而是返回一个**协程对象**。

协程对象注册到事件循环，由**事件循环调用**。

### loop:

`loop = asyncio.get_event_loop()`

创建一个事件循环

`run_until_complete(future_obj)`

它的参数是Future对象

它将协程coroutine封装成task对象（Future的子类）。

将协程注册到事件循环，并对协程加以封装，包含任务的各种状态。

`asyncio.ensure_future()`和`asyncio.create_task()`都可以创建一个task。

`asyncio.wait(tasks) `接受一个tasks列表

`asyncio.gather(*task) `接受一堆task

### task任务：

一个协程对象就是一个原生可以挂起的函数，任务就是对协程的进一步封装，其中包含任务的各种状态。

其状态包括：

- `pending` task创建时的状态
- `running `执行时候的状态
- `done` 调用完毕
- `cancelled`

**future** 代表将来执行或者没有任务的结果，和task没有本质的区别。

创建task后，task在加入事件循环之前都是pedding状态，当其在事件循环中执行完毕后是finished状态。

### await:

使用async可以定义协程，使用await可以针对耗时的操作进行挂起，就像生成器里的yield函数让出控制权。协程遇到await，事件循环挂起该协程，执行别的协程。

可以用 `asyncio.sleep()` 模拟协程耗时操作。

```python
import asyncio,time
async def work():
    await asyncio.sleep(5)
start = time.time()
loop = asyncio.get_event_loop()
coroutine1 = work()
coroutine2 = work()
coroutine3 = work()
tasks = [
    asyncio.ensure_future(coroutine1),
    asyncio.ensure_future(coroutine2),
    asyncio.ensure_future(coroutine3)
]
loop.run_until_complete(asyncio.wait(tasks))
loop,close()
end = time.time()
print(end-start)
```

输出：

```python
5.005608081817627
```

### 协程套嵌：

即在一个协程中await另外一个协程。

