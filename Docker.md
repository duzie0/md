# Docker

## 基本概念：

开源的软件部署解决方案；

轻量级的应用容器框架；

用来打包、发布、运行任何的应用；

Docker是一个开源的引擎，可以轻松的为任何应用创建一个轻量级的、可移植的、自给自足的容器。开发者在笔记本上编译测试通过的容器可以批量地在生产环境中部署，包括VMs（虚拟机）、bare metal、OpenStack 集群和其他的基础应用平台。

## 适用场景：

- web应用的自动化打包和发布；
- 自动化测试和持续集成、发布；
- 在服务型环境中部署和调整数据库或其他的后台应用；
- 从头编译或者扩展现有的OpenShift或Cloud Foundry平台来搭建自己的PaaS环境。

## Docker组件：

**docker server服务端**

客户端————>向docker服务器进程发起请求，如:创建、停止、销毁容器等操作

**docker client客户端**

服务器进程—–>处理所有docker的请求，管理所有容器

**docker registry 镜像仓库**

镜像仓库——>镜像存放的中央仓库，可看作是存放二进制的scm

## Docker的优缺点：

### 优点：（轻量、快速、隔离）

1.一些优势和VM一样，但不是所有都一样。
比VM小，比VM快，Docker容器的尺寸减小相比整个虚拟机大大简化了分布到云和从云分发。
2.对于在笔记本电脑，数据中心的虚拟机，以及任何的云上，运行相同的没有变化的应用程序，IT的发布速度更快。
Docker是一个开放的平台，构建，发布和运行分布式应用程序。
Docker使应用程序能够快速从组件组装和避免开发，QA和生产环境之间的摩擦。
3.您可以在部署在公司局域网或云或虚拟机上使用它。
4.开发人员并不关心具体哪个Linux操作系统
使用Docker，开发人员可以根据所有依赖关系构建相应的软件，针对他们所选择的操作系统。
然后，在部署时一切是完全一样的，因为一切都在DockerImage的容器在其上运行。
开发人员负责并且能够确保所有的相关性得到满足。
5.Google，微软，亚马逊，IBM等都支持Docker。

### 缺点：

1.Docker支持Unix/Linux操作系统，不支持Windows或Mac（即使可以在其上安装，不过也是基于Linux虚拟机的）
2.Docker用于应用程序时是最有用的，但并不包含数据。日志，跟踪和数据库等通常应放在Docker容器外。

## Docker的其他概念：

### 容器：

docker容器可以理解为在沙盒中运行的进程。这个沙盒包含了该进程运行所必须的资源，包括文件系统、系统类库、shell 环境等等。但这个沙盒默认是不会运行任何程序的。你需要在沙盒中运行一个进程来启动某一个容器。这个进程是该容器的唯一进程，所以当该进程结束的时候，容器也会完全的停止。

容器一旦被直接推出，之前安装的gcc啊vim啊啥的就会全部gg掉。如果要保存修改，就需要将当前容器封装成一个新的镜像，这样下次启动这个新的镜像后之前作出的修改还都在。

### 镜像：

镜像是静态的、可以被用户互相分享的文件。我们玩过双系统和虚拟机的人都知道，首先你需要一个.iso镜像，才能安装系统。Docker中的镜像也是这个东西，镜像是静态的，你不能对他操作，只能pull别人的镜像或者push自己的镜像。

### 仓库：

Docker中的仓库很像git的代码仓库，你可以pull自己之前push到自己仓库的镜像到本地，也可以pull别人push到公共仓库的镜像到自己本地。

## Docker命令：

**查看版本：**`docker version`

**搜索可用的docker镜像:**`docker search <镜像名字>`  `docker search tutorial`

**下载镜像：**`docker pull <镜像名字>` `docker pull learn/tutorial`

**在容器中安装新程序：**`docker run learn/tutorial apt-get install -y ping`

**获得安装完ping命令之后容器的id:**`docker ps -l`

**查看所有的镜像列表：**`docker ps`

**列出所有安装过的镜像：**`docker images`

**查看commit的参数列表:**`docker commit`

**保存对容器的修改:**`docker commit 698 learn/ping` 698是id，learn/ping 新的镜像名

**运行新的镜像**：`docker run learn/ping ping www.baidu.com`

**检查运行中的镜像:**`docker inspect <镜像id>`

**发布docker镜像：**`docker push <镜像名>`

后台启动一个容器后，如果想进入到这个容器，可以使用attach命令：

`docker attach container_name/container_id`

**启动、停止、重启容器命令：**

- `docker start container_name/container_id`
- `docker stop container_name/container_id`
- `docker restart container_name/container_id`

**删除镜像：**

- 删除镜像前必须先删除以此镜像为基础的容器。
- `docker rm container_name/container_id`
- `docker rmi image_name/image_id`

**参数：**

- `-i`表示打开并保持stdout
- `-t`表示分配一个终端（pseudo-tty）
- `-it` 是以上两个参数，此时如果使用exit退出，则容器的状态处于Exit，而不是后台运行。
- 如果想让容器一直运行，而不是停止，可以使用快捷键 ctrl+p ctrl+q 退出，此时容器的状态为Up。
- `-d` 后台运行

## 新建自有镜像：

### 第一种：

构建自己的镜像是需要基础镜像的，比如Ubuntu

拉取镜像：`docker pull ubuntu`

运行ubuntu：`docker run -it ubuntu /bin/bash`

### 第二种，通过Dockerfile创建：

Dockerfile可以理解为一种配置文件，用来告诉docker build命令应该执行哪些操作。

```python
#说明该镜像是以哪个镜像为基础的
FROM ubuntu:latest
# 构建者的基本信息
MAINTAINER duzie

# 在build这个镜像时执行的操作
RUN apt update
RUN apt install git

# 拷贝本地文件到镜像中
COPY ./* /usr/share/gitdir/
```

用build命令构建镜像：

`docker build -t='duzie/ubuntu:gitdir' .` -t用来指定新镜像的用户信息、tag等

**镜像的更新也有两种：**

- 创建容器之后做更改，之后commit生成镜像，然后push到仓库中。
- 更新Dockerfile。在工作时一般建议这种方式，更简洁明了。

