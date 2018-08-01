# uWSGI

## WSGI (Web Server Gateway Interface)

是一个Web服务器(如Nginx)与应用服务器（uWSGI）通信的一种**规范（协议）**。

在生产环境中使用WSGI作为python web的服务器。Python Web服务器网关接口，是Python应用程序或框架和Web服务器之间的一种接口，被广泛接受。WSGI没有官方的实现, 因为WSGI更像一个协议，只要遵照这些协议，WSGI应用(Application)都可以在任何服务器(Server)上运行。

## uWSGI

uWSGI是一个web服务器，实现了WSGI协议、uwsgi协议、http协议等。

uWSGI实现了WSGI的所有接口，是一个快速、自我修复、开发人员和系统管理员友好的**服务器**。uWSGI代码完全用C编写，效率高、性能稳定。

uwsgi是一种线路协议而不是通信协议，在此常用于在uWSGI服务器与其他网络服务器的数据通信。uwsgi协议是一个uWSGI服务器自有的协议，它用于定义传输信息的类型。

## 作用

Django 是一个 Web 框架，框架的作用在于处理 request 和 response，其他的不是框架所关心的内容。所以怎么部署 Django 不是 Django 所需要关心的。

Django 所提供的是一个开发服务器，这个开发服务器，没有经过安全测试，而且使用的是 Python 自带的 simple HTTPServer 创建的，在安全性和效率上都是不行的。

而uWSGI 是一个全功能的 HTTP 服务器，他要做的就是把 HTTP 协议转化成语言支持的网络协议。比如把 HTTP 协议转化成 WSGI 协议，让 Python 可以直接使用。 
**uwsgi** 是一种 uWSGI 的内部协议，使用二进制方式和其他应用程序进行通信。

## uWSGI安装与配置

### ubuntu下安装：

`apt install uwsgi`

#### uWSGI配置:

`touch app.ini`

```uWSGI
[uwsgi]
#指定module
module = wsgi:app
#并发的进程数
processes = 4
http = 127.0.0.1:5000
master = true
#具体的文件名
#socket = mysite.sock
#socket文件读写权限
#chmod-socket = 666
#socket = 127.0.0.1:8889
```

## Nginx+uWSGI:

/home/hsc/Django/mysite.sock

Nginx和uWSGI通过mysite.sock传递数据

### Nginx配置：

```nginx
server{
  listen 80;
  server_name 127.0.0.1;
  #动态资源
  location / {
    #proxy_pass http://127.0.0.1:5000;
    #导入uwsgi_params
    include uwsgi_params;
    #指定uwsgi代理
    uwsgi_pass unix:/home/hsc/Django/mysite.sock;
  }
  #静态资源
  location /static/ {
    alias /home/hsc/Django/static/;
  }
  location /templates/ {
    alias /home/hsc/Django/templates/;
  }
    location /media/ {
    alias /home/hsc/Django/media/;
  }
}
```

### uWSGI配置：

```uWSGI
[uwsgi]
module = wsgi:app
processes = 4
#http = 127.0.0.1:5000
socket = mysite.sock
#另一种指定socket方法
#socket = 127.0.0.1:8123
#设置socket权限
chmod-socket = 666
```

## 运行

`uwsgi app.ini`

