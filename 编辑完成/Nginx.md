# Nginx

Nginx 是一个高性能的 Web 和反向代理服务器, 它具有有很多非常优越的特性:

## 作为 Web 服务器

相比 Apache，Nginx 使用更少的资源，支持更多的并发连接，体现更高的效率，这点使 Nginx 尤其受到虚拟主机提供商的欢迎。能够支持高达 50,000 个并发连接数的响应，感谢 Nginx 为我们选择了 epoll and kqueue 作为开发模型。

## 作为负载均衡服务器

Nginx 既可以在内部直接支持 Rails 和 PHP，也可以支持作为 HTTP代理服务器 对外进行服务。Nginx 用 C 编写, 不论是系统资源开销还是 CPU 使用效率都比 Perlbal 要好的多。

## 作为邮件代理服务器

Nginx 同时也是一个非常优秀的邮件代理服务器（最早开发这个产品的目的之一也是作为邮件代理服务器），Last.fm 描述了成功并且美妙的使用经验。

## Nginx 安装非常的简单，配置文件 非常简洁（还能够支持perl语法），Bugs非常少的服务器

Nginx 启动特别容易，并且几乎可以做到7*24不间断运行，即使运行数个月也不需要重新启动。你还能够在 不间断服务的情况下进行软件版本的升级。

## Nginx的优点是：

1、工作在网络的7层之上，**可以针对http应用做一些分流的策略**，比如针对域名、目录结构，它的正则规则比HAProxy更为强大和灵活，这也是它目前广泛流行的主要原因之一，Nginx单凭这点可利用的场合就远多于LVS了。

2、**Nginx对网络稳定性的依赖非常小，理论上能ping通就就能进行负载功能**，这个也是它的优势之一；相反LVS对网络稳定性依赖比较大，这点本人深有体会；

3、**Nginx安装和配置比较简单，测试起来比较方便，它基本能把错误用日志打印出来。**LVS的配置、测试就要花比较长的时间了，LVS对网络依赖比较大。

3、**可以承担高负载压力且稳定**，在硬件不差的情况下一般能支撑几万次的并发量，负载度比LVS相对小些。

4、**Nginx可以通过端口检测到服务器内部的故障，**比如根据服务器处理网页返回的状态码、超时等等，并且会把返回错误的请求重新提交到另一个节点，不过其中**缺点就是不支持url来检测。**比如用户正在上传一个文件，而处理该上传的节点刚好在上传过程中出现故障，Nginx会把上传切到另一台服务器重新处理，而LVS就直接断掉了，如果是上传一个很大的文件或者很重要的文件的话，用户可能会因此而不满。

5、Nginx不仅仅是一款优秀的负载均衡器/反向代理软件，它同时也是功能强大的Web应用服务器。LNMP也是近几年非常流行的web架构，在高流量的环境中稳定性也很好。

6、Nginx现在作为Web反向加速缓存越来越成熟了，速度比传统的Squid服务器更快，可以考虑用其作为反向代理加速器。

7、Nginx可作为中层反向代理使用，这一层面Nginx基本上无对手，唯一可以对比Nginx的就只有lighttpd了，不过lighttpd目前还没有做到Nginx完全的功能，配置也不那么清晰易读，社区资料也远远没Nginx活跃。

8、Nginx也可作为静态网页和图片服务器，这方面的性能也无对手。还有Nginx社区非常活跃，第三方模块也很多。

淘宝的前端使用的Tengine就是基于nginx做的二次开发定制版。

## Nginx的缺点是：

1、Nginx仅能支持http、https和Email协议，这样就在适用范围上面小些，这个是它的缺点。

2、对后端服务器的健康检查，只支持通过端口来检测，不支持通过url来检测。不支持Session的直接保持，但能通过ip_hash来解决。

## Nginx安装与配置

## **ubuntu**下安装：

#### ubuntu下安装：

`sudo apt install nginx`

#### 进入nginx安装目录：

`cd /etc/nginx/`

#### 在sites-available下配置项目：

`touch sites-available/django`

`sudo vim sites-available/django`

```nginx
server{
  #监听80端口
  listen 80;
  #服务名，域名
  server_name 10.31.161.16;
  charset UTF-8;
  # 日志文件
  access_log /hsc/access.log;
  error_log /hsc/error.log;
  client_max_body_size 75M;
  #动态资源
  #只要是http://127.0.0.1/api都匹配到下面的配置
  location /api{
    #include uwsgi_params;
    #代理，转到5000端口（网关）
    proxy_pass http://127.0.0.1:5000;
    #uwsgi_pass unix:/home/hsc/Django;
    uwsgi_pass 127.0.0.1:8888;
    #链接超时时间
    uwsgi_read_timeout 30;
  }
  
  #静态资源
  location / {
    #root指定的是静态网页的根目录
    root /home/hsc/Django/static/;
    #index指定的是
    index index.html index.htm;
  }
}
```

#### 创建项目的软连接：

这里的软链接要写绝对路径，不然会报错

`sudo ln -s /etc/nginx/sites-available/django /etc/nginx/sites-enabled/` 

在sites-enabled中编辑配置，实际上是在sites-available中编辑配置信息

##### 不在sites-enabled目录下直接编写配置文件的原因是：

sites-enabled放要启用的网站，sites-available放网站配置；

使用vim编辑时出现异常会保存`.aaa.swp` 隐藏文件，一旦出现这种情况nginx会读取`.aaa.swp` 作为配置信息。执行错误配置信息，服务器无法启动。

#### 重启Nginx服务

`sudo nginx -s reload`







