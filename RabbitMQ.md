# RabbitMQ

## 基本概念：

**RabbitMQ**

**是实现AMQP（高级消息队列协议）的消息中间件**的一种，服务器端用Erlang语言编写，支持多种客户端，如：Python、Ruby、.NET、Java、JMS、C、PHP、ActionScript、XMPP、STOMP等，支持AJAX。用于在 **分布式系统中存储转发消息。**

**AMQP**

AMQP，即**Advanced Message Queuing Protocol，高级消息队列协议**，是应用层协议的一个开放标准，为面向消息的中间件设计。它从生产者接收消息并递送给消费者，在这个过程中，根据规则进行路由，缓存与持久化。

而在AMQP中主要有**两个组件：Exchange 和 Queue** （在 AMQP 1.0 里还会有变动），如下图所示，绿色的 X 就是 Exchange ，红色的是 Queue ，这两者都在 Server 端，又称作 Broker ，这部分是 RabbitMQ 实现的，而蓝色的则是客户端，通常有 Producer 和 Consumer 两种类型：

![idoall.org](http://o6y6ob8ky.bkt.clouddn.com/rabbitmq-2.png-1)

## RabbitMQ的基础概念

- **Broker**：简单来说就是消息队列服务器实体
- **Exchange**：消息交换机，它指定消息按什么规则，路由到哪个队列
- **Queue**：消息队列载体，每个消息都会被投入到一个或多个队列
- **Binding**：绑定，它的作用就是把exchange和queue按照路由规则绑定起来
- **Routing Key**：路由关键字，exchange根据这个关键字进行消息投递
- **vhost**：虚拟主机，一个broker里可以开设多个vhost，用作不同用户的权限分离
- **producer**：消息生产者，就是投递消息的程序
- **consumer**：消息消费者，就是接受消息的程序
- **channel**：消息通道，在客户端的每个连接里，可建立多个channel，每个channel代表一个会话任务

## 安装RabbitMQ：

#### ubuntu下安装：

**安装Erlang：**`sudo apt install erlang`

**安装RabbitMQ Server**  `sudo apt install rabbitmq-server`

**签署RabbitMQ版本的密钥添加到apt-key：**`wget -O  - 'https : //dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc'| sudo apt-key add  - `

**导入key ：**`sudo rpm --import http://www.rabbitmq.com/rabbitmq-signing-key-public.asc`

可先将内容保存至文本文件，如，rabbitmq-signing-key-public.asc.txt

`sudo rpm --import rabbitmq-signing-key-public.asc.txt `

`sudo apt install rabbitmq-server-3.4.1-1.noarch.rpm`

注册为系统服务： `sudo chkconfig rabbitmq-server on`

启动RabbitMQ Server 

https://www.cnblogs.com/liuchuanfeng/p/6813205.html

