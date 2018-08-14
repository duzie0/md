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

yield from 后面必须跟iterable对象，一般都是耗时的操作

### **async **:

**async无法将一个生成器转化为协程。**

async定义一个协程，协程是一种对象，需要加入事件循环loop才能运行。

async标记的协程调用**不会立刻执行**，而是返回一个**协程对象**。

协程对象注册到事件循环，由**事件循环调用**。

### loop:

loop = asyncio.get_event_loop()

创建一个事件循环

run_until_complete(future_obj)

它的参数是Future对象

它将协程coroutine封装成task对象（Future的子类）。

将协程注册到事件循环，并对协程加以封装，包含任务的各种状态。

asyncio.ensure_future()和asyncio.create_task()都可以创建一个task。

asyncio.wait(tasks) 接受一个tasks列表

asyncio.gather(*task) 接受一堆task