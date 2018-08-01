# Django

## 1.注意

​	开始一个新的项目使用虚拟环境来隔离不同的项目

​	pip install virtualenv

​	virtualenv venv  --python=python3

​	source venv/bin/activate

​	安装Django

​	pip install django==1.11	

## 2.Django的一些命令操作

#### 	2.1.创建项目

​		django-admin startproject helloword

#### 	2.2.运行&测试

​		python manage.py runserver 0.0.0.0:8000

​		python manage.py runserver 

#### 	2.3.创建App

​		python manage.py startproject helloword

#### 	2.4.迁移数据库

​		python manage.py makemigrations

​		python manage.py migrate     #创建表

#### 	2.5.admin模块，创建超级用户

​		python manage.py createsuperuser



## 3.Django :配置、模型、模板、视图、admin

### 3.1.配置settings：

​	ALLOWED_HOSTS：允许访问的ID

​	INSTALLED_APPS：安装创建的APP

​	TEMPLATES：'DIRS': [os.path.join(BASE_DIR, 'templates')]	

​	WSGI_APPLICATION = 'Django.wsgi.application'

###### 	MySql配置：

​			DATABASES = {   

​					 'default': {        

​						'ENGINE': 'django.db.backends.mysql',       

​						'NAME': 'blog',        

​						'HOST':'127.0.0.1',        

​						'USER':'root',        

​						'PORT':'3306',        

​						'PASSWORD':'123456',    }				

​			}

###### 	Redis配置：

    CACHES = {
    	"default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/1",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
       	 }
    	}
    }
​	静态资源配置：STATIC_URL = '/static/'

​					   STATICFILES_DIRS = [    os.path.join(BASE_DIR,'static'),]

​	MEDIA资源配置：MEDIA_URL = '/media/photos/'

​				       MEDIA_ROOT = 'media/photos'

​					urls.py: urlpattern = [] 

​						+static(settings.MEDIA_URL,document_root=settings.MEDIA_ROOT)

​	LANGUAGE_CODE = 'zh-hans'  英文改为中文

​	TIME_ZONE = 'Asia/Shanghai'	UTC时区

​	USE_TZ = False	改为False，否则数据库存储使用UTC时间

### 3.2.模型models：

​	创建继承自Django Model的模型类，制定各个字段的属性

#### 	Django的字段：

​	username = models.CharField(max_length=150,unique=True)

​	age = models.IntegerField(default=10)

​	email = models.EmailField(default='aaaa@gmail.com')

​	sex = models.BooleanField(default=True)

​	created_at = models.DateTimeField(auto_now_add=True)	

​	img = models.ImageField(upload_to='photo/',default='default/default.jpg')

​	content = models.TextField()	

#### 	数据库表之间的关系：

​	一对一：OneToOneField

​			place = models.OneToOneField(Place,on_delete=models.CASCARD,primary_key=True)

​			on_delete = models.CASCARD 级联删除

​	一对多：

​			author = models.ForeignKey(User)

​	多对多：

#### 	增删改查CRUD：

###### 	创建create：

​		user = User(username = '张三')

​		user.save()

​		User.objects.create(username = '张三')

###### 	查询Retrieve：

​		users = User.objects.all()

​		条件查询：

​			user.objects.filter(id_gt=2)

​			_gt:大于

​			_lt:小于

​			_gte:大于等于

​			_lte:小于等于

​			_contains:包含

​			_regex:使用正则表达式过滤

###### 	更新Update：

​			user = User.objects.get(id = 1)

​			user.age = 20

​			user.save()

###### 	删除Delete：

​		user = User.objects.get(id = 1)

​		user.delete()

​		users = User.objects.filter(id_gt=20)

​		users.delete()

### 3.3.模板templates：

​	继承模板：{% extends 'base.html' %}

​	导入模板：{% include 'navbar.html' %}	

​	载入static: {% load 'static' %}

​	留block:     {% block blockname%}

​			    {% endlblock %}

### 3.4.视图views：

​	视图是后端数据的控制器

​	根据匹配的路由执行对应的方法

​	返回值即是对客户端请求的响应	

​	开发中view的写法一般使用Restful

###### Restful的写法

```python
#API 接口
class API(object):
    def __init__(self,version='v1'):
        “”“
        version:版本
        ”“”
        self.version = version
        self.resource = []
        
     def add_source(self,resource):
        “”“
        resource:Resource对象
        ”“”
        self.resource.append(resource)
      
      @property
      def urls(self):
        urlpatterns = [
          url(r'^/{}/{}/$'.format(self.version,resource.name),resource.process_method)
            for resource in self resources
        ]
        return urlpatterns

#Resource 类
class Resource(object):
    def __init__(self,name=None)
    	self.name = name or self.__class__.__name__.lower()
        
    def process_method(self,request,*args,**kwargs):
        method = request.method
        if method == 'GET':
            return self.get(self,request,*args,**kwargs)
        if method == 'POST':
            return self.post(self,request,*args,**kwargs)
        if method == 'PUT':
            return self.put(self,request,*args,**kwargs)
        if method == 'DELETE':
            return self.delete(self,request,*args,**kwargs)
        
     def get(self,request,*args,**kwargs):
        return HttpResponse('GET')
     def post(self,request,*args,**kwargs):
        return HttpResponse('POST')
     def put(self,request,*args,**kwargs):
        return HttpResponse('PUT')
     def delete(self,request,*args,**kwargs):
        return HttpResponse('DELETE')

#User类，继承Resource,重写get,post,put,delete方法
#实现根据同一个url的不同请求方式匹配不同的视图方法
#在各自的对应的请求方法中写相应的逻辑
class User(Resource):
    def get(self,request,*args,**kwargs):
        return HttpResponse('get')
    def post(self,request,*args,**kwargs):
        return HttpResponse('post')
    def delete(self,request,*args,**kwargs):
        return HttpResponse('delete')
    def put(self,request,*args,**kwargs):
        return HttpResponse('put')
 
class Book(Resource):
    def get(self,request,*args,**kwargs):
        return HttpResponse('get')
    def post(self,request,*args,**kwargs):
        return HttpResponse('post')
    def delete(self,request,*args,**kwargs):
        return HttpResponse('delete')
    def put(self,request,*args,**kwargs):
        return HttpResponse('put')

#二级路由配置
api = API(version='v1.0')
user = User(name='user')
book = Book(name='book')
api.add_resource(user)
api.add_resource(book)

#主路由配置
from django.conf.urls import url,include

urlpatterns = [
  url(r"^app/",include(api.urls)),
] 
```

### 3.5.后台管理admin

#### 	django_admin:

​		admin对应的数据表：auth_groups

##### 	应用注册

​		在admin.py中实现应用注册

```python
from django.contrib import admin
from blog.models import Blog
#Blog模型管理器
class BlogAdmin(Blog,BlogAdmin):
    list_display = ('id','caption','author','created_at')
#在admin中绑定注册  
admin.site.register(Blog,BlogAdmin)
-------------------------------------------------------------
@admin.register(Blog)
class BlogAdmin(Blog,BlogAdmin):
    list_display = ('id','caption','author','created_at')
```

##### 	记录列表基本设置

```python
#list_display 设置要显示在列表中的字段
list_display = ('id','author','created_at')
#设置每页显示多少条记录，默认100条
list_per_page = 50
#ordering 设置默认排序字段，负号表示降序
ordering = ('-publish_time')
#list_editable 设置默认可编辑字段
list_editable = ['content','summary']
#fk_fields设置显示外键字段
fk_fields = ('machine_id')
#设置哪些字段可以点击进入编辑界面
list_display_links = ('id','caption')
```

##### 	筛选器

```python
from django.contrib import admin
from blog.models import Blog
  
#Blog模型的管理器
@admin.register(Blog)
class BlogAdmin(admin.ModelAdmin):
    list_display = ('id', 'caption', 'author', 'publish_time')
     
    #筛选器
    list_filter =('trouble', 'go_time', 'act_man__user_name', 'machine_room_id__machine_room_name') #过滤器
    search_fields =('server', 'net', 'mark') #搜索字段
    date_hierarchy = 'go_time'    # 详细时间分层筛选　
```

一般ManyToManyField多对多字段用过滤器；

标题等文本字段用搜索框；

日期时间用分层筛选。

过滤器如果是外键需要遵循这样的语法：本表字段外键表要显示的字段。如：“user_user_name”

##### 颜色显示

写在models.py中

```python
from django.db import models
from django.contrib import admin
from django.utils.html import format_html
 
class Person(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    color_code = models.CharField(max_length=6)
 
    def colored_name(self):
        return format_html(
            '<span style="color: #{};">{} {}</span>',
            self.color_code,
            self.first_name,
            self.last_name,
        )
 
class PersonAdmin(admin.ModelAdmin):
    list_display = ('first_name', 'last_name', 'colored_name')
```

##### 页面标题和头部内容

#### 	django_xadmin:

​		xadmin地址：https://github.com/sshwsfc/xadmin

​		pip安装支持Python 2.x,不支持Python 3.x : pip install git+git://github.com/sshwsfc/xadmin.git

###### 		源码安装：

​		下载xadmin文件

​		pip安装xadmin:pip install xadmin

​		pip卸载xadmin:pip uninstall xadmin    不会卸载掉相应的依赖

​		拷贝安装xadmin源码：在项目中新建extra_apps文件夹，保存第三方库，将xadmin拷贝进去

​		注册extra_apps到source：pycharm：make source root

​		将xadmin添加到settings.py应用列表：'xadmin', 'crispy_forms',

​		使用migrate同步数据表	

​		在urls.py中进行xadmin的配置:url(r'^xadmin/',xadmin.site.urls)		

​		启动服务

​		刷新数据库：python manage.py flush

​		创建用户：python manage.py createsuperuser





​		

​		

​			