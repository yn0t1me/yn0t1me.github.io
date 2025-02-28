---
title: "Django"
slug: "Django"
description: "python后端开发常用框架：Django"
date: "2025-02-27T18:10:47+08:00"
lastmod: "2025-02-27T18:10:47+08:00"
image: 
categories: ["python"]
tags: ["Django","python"]
---


<!--more-->

## Django项目创建

项目+虚拟环境

### Django项目的创建

```python
# 安装指定版本的djano
pip install django==3.2
```

#### 命令行操作

```python
cd 项目目录
django-admin startproject 项目名
```

项目结构

```python
myproject
├── manage.py              [项目的管理工具]  
└── myproject
    ├── __init__.py
    ├── settings.py        【配置文件，只有一部分。程序启动时，先读取django内部配置，再读settings.py】
    ├── urls.py			   【主路由，在里面编写  /xxx/xxx/xxx ---> index 】
    ├── asgi.py            【异步】
    └── wsgi.py            【同步，主】
```

项目启动

```python
cd 项目名
python manage.py runserver IP:端口（可默认不写）
```

app

```python
cd 项目
python manage.py startapp app名
```

项目结构

```python
myproject
├── manage.py
├── myproject
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── web
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── migrations
    │   └── __init__.py
    ├── models.py
    ├── tests.py
    └── views.py
```

#### Pycharm操作
创建django项目

![](/post/Python/Django/1732088054327-ef4f3362-451c-4611-8d64-2ba238d83fb0.png)

生成效果

![](/post/Python/Django/1732088219082-78f02080-3e2f-4b4f-8fee-60bcef043724.png)

### 虚拟环境
使用虚拟环境可以是各个项目之间的环境隔离开，为每一个项目单独创建一个虚拟环境，而不是使用系统解释器

#### 命令行
此处只介绍virtualenv的用法

```python
# 首先pip安装virualenv库 随便哪个系统解释器均可
pip install virtualenv

cd 目标文件夹
virtualenv 虚拟环境名 --python=系统解释器

virtualenv /目标文件夹/虚拟环境名 --python=系统解释器
```

操作

+ 将D:\envs视为以后存放虚拟环境的目录

```python
cd \envs
virtualenv crm --python=python39
```

![](/post/Python/Django/1732089006310-a4f0e644-3969-49ec-94b1-82bed5401f53.png)

+ 激活虚拟环境

```python
cd crm\Scripts
activate
```

出现图示(虚拟环境名)表明激活成功

![](/post/Python/Django/1732089138522-4e67f8fc-51a0-4114-9e2e-50c28c2b5989.png)

+ 安装包

```python
pip install 包名
```

+ 创建django项目

```python
cd 项目目录
django-admin startproject crm
```

![](/post/Python/Django/1732089413412-38b9f663-f1e6-4e9b-be4d-0336f8867975.png)

```python
python manage.py startapp xxx
python manage.py runserver
```

+ 退出虚拟环境

```python
deactivate
```

#### Pycharm
创建指定django版本的项目

+ 创建纯净项目+创建虚拟环境

![](/post/Python/Django/1732089953446-ec7f2c26-9685-4617-a075-302bc5c97ba1.png)

+ 创建django项目

安装django

```python
pip install django==3.2
```

创建项目

```python
django-admin startproject myproj
```

因为我的pycharm虚拟环境有问题，只有在Script目录下才被激活，因此指定了一下创建路径

![](/post/Python/Django/1732090258231-79603868-d786-4ff9-840e-97f7297771a0.png)

+ 配置django的启动按钮

![](/post/Python/Django/1732091034828-6dd33792-a724-4923-85b1-042177510b1a.png)

fix键

![](/post/Python/Django/1732091520209-c45cff8d-7cfb-495e-ab4a-471cae5abb49.png)

![](/post/Python/Django/1732091447518-e3c10327-4abf-458a-86ac-f4a432e468f0.png)

### app创建
单个app目录结构如下

```python
myproj
├── manage.py
├── myproj
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── web
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── migrations
    │   └── __init__.py
    ├── models.py
    ├── tests.py
    └── views.py
```

要使用多app时，建议在根目录建立apps目录，在其中依次建立以app名命名的目录

```python
myproj
	.venv
    myproj
    	...
        ...
    manage.py
    apps
    	web
        backend
        api
```

```python
# 先创建如上结构的目录
python manage.py startapp spi apps/api
python manage.py startapp web apps/web
python manage.py startapp backend apps/backend
```

### 纯净版
![](/post/Python/Django/1732091798025-cc0487a7-6d8c-4a3f-9dfb-3105790af457.png)

将不需要使用的app，中间件，模板在settings文件中注释即可得到一个纯净版的django项目



## Django项目文件结构及作用（个人习惯）
```python

D:\Data\Codefield\CODE-Python\py全栈\模块六\day06>treee -I .venv -f
.
|-- ./day06
|   |-- ./day06/__init__.py
|   |-- ./day06/__pycache__
|   |   |-- 省略
|   |-- ./day06/asgi.py
|   |-- ./day06/local_settings.py【本地配置，如数据库等配置，区别于settings】
|   |-- ./day06/settings.py
|   |-- ./day06/urls.py【书写路由与视图函数对应关系】
|   `-- ./day06/wsgi.py
|-- ./django_error.log 【错误日志】
|-- ./manage.py
|-- ./requirements【库依赖文件】
|-- ./scripts【脚本文件，随意写，放杂物】
|   |-- ./scripts/init_admin.py
|   |-- ./scripts/init_customer.py
|   `-- ./scripts/\303\346\317\362\266\324\317\363\274\314\263\320.py
|-- ./shell【部署时方便操作的bash脚本】
|   |-- ./shell/reboot.sh
|   |-- ./shell/shop.sh
|   `-- ./shell/uwsgi_day06.ini
|-- ./utils【小的功能库】
|   |-- ./utils/__pycache__
|   |   |-- 略
|   |-- ./utils/aliyun.py【短信收发】
|   |-- ./utils/bootstrap.py【bootstrap样式渲染脚本】
|   |-- ./utils/encrypt.py【密码加解密】
|   |-- ./utils/group.py
|   |-- ./utils/link.py
|   |-- ./utils/md.py
|   |-- ./utils/pager.py【分页组件】
|   |-- ./utils/response.py【response类，用于方便构建返回值】
|   `-- ./utils/video.py【获取视频原播放量的脚本】
`-- ./web【单app】
    |-- ./web/__init__.py
    |-- ./web/__pycache__
    |   |-- 略
    |-- ./web/admin.py
    |-- ./web/apps.py
    |-- ./web/forms【编写form组件用于生成表单，校验提交信息】
    |   |-- ./web/forms/__pycache__
    |   |   `-- ./web/forms/__pycache__/account.cpython-39.pyc
    |   `-- ./web/forms/account.py
    |-- ./web/migrations【数据库操作记录文件】
    |   |-- ./web/migrations/0001_initial.py
    |   |-- 略
    |-- ./web/models.py【编写数据库结构，字段等等】
    |-- ./web/static【静态文件存放目录】
    |   |-- ./web/static/css
    |   |   |-- ./web/static/css/commons.css
    |   |   |-- 略
    |   |-- ./web/static/images
    |   |   `-- ./web/static/images/default.png
    |   |-- ./web/static/js
    |   |   |-- ./web/static/js/JQuery3.6.0.js
    |   |   |-- 略
    |   `-- ./web/static/plugins
    |       |-- ./web/static/plugins/bootstrap
    |       |   |-- ./web/static/plugins/bootstrap/css
    |       |   |-- ./web/static/plugins/bootstrap/fonts
    |       |   `-- ./web/static/plugins/bootstrap/js
    |       `-- ./web/static/plugins/font-awesome
    |           |-- 略
    |-- ./web/templates
    |   |-- ./web/templates/form.html【表单显示模板】
    |   |-- ./web/templates/form2.html【表单显示模板2】
    |   |-- ./web/templates/home.html【各种页面】
    |   |-- ./web/templates/include【include】
    |   |   |-- ./web/templates/include/delete_modal.html
    |   |   `-- ./web/templates/include/search_group.html
    |   |-- ./web/templates/layout.html【用于继承的模板文件】
    |   |-- ./web/templates/tag【tag】
    |   |   `-- ./web/templates/tag/my_menu.html
    |   `-- ./web/templates/transaction_list.html
    |-- ./web/templatetags【处理复杂页面渲染逻辑】
    |   |-- ./web/templatetags/__pycache__
    |   |   |-- 略
    |   |-- ./web/templatetags/color.py
    |   |-- ./web/templatetags/menu.py
    |   `-- ./web/templatetags/permission.py
    |-- ./web/tests.py
    `-- ./web/views【视图函数，处理逻辑】
        |-- ./web/views/__pycache__
        |   |-- 略
        |-- ./web/views/account.py
        |-- ./web/views/customer.py
        |-- ./web/views/level.py
        |-- ./web/views/my_order.py
        |-- ./web/views/my_transaction.py
        `-- ./web/views/policy.py
```

经典的MVC设计模式及其优点：

MVC即 Model-View-Controller(模型-视图-控制器) ，是经典的软件开发设计模式

+ **Model (模型)** ： 简而言之即数据模型。模型不是数据本身（比如数据库里的数据），而是抽象的描述数据的构成和逻辑关系。通常模型包括了数据表的各个字段（比如人的年龄和出生日期）和相互关系（单对单，单对多关系等)。Web开发框架会根据模型的定义来自动生成数据表。
+ **View (视图)**： 主要用于显示数据，用来展示用户可以看到的内容或提供用户可以输入或操作的界面。数据来源于哪里？当然是数据库啦。那么用户输入的数据给谁? 当然是给控制器啦。
+ **Controller(控制器)**：应用程序中处理用户交互的部分。通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据（比如增加或更新数据表）。

MVC设计模式实现了软件或网络应用开发过程中数据、业务逻辑和用户界面的分离，使软件开发更清晰，也是维护变得更容易。这与静态网页设计中使用html和css实现了内容和样式的分离是同一个道理。



Django的MVT设计模式由Model模型，View（视图）和Template（模板）三部分组成，分别对应单个app目录下的models.py, views.py和templates文件夹

> 因实际开发时view函数过多，为了方便管理通常使用views目录取代单个view.py文件，在views目录下按功能编写各个py文件
>

+ **Django Model(模型)**: 这个与经典MVC模式下的模型Model差不多。
+ **Django View(视图)**: 这个与MVC下的控制器Controller更像。视图不仅负责根据用户请求从数据库读取数据、指定向用户展示数据的方式(网页或json数据), 还可以指定渲染模板并处理用户提交的数据。
+ **Django Template(模板)**: 这个与经典MVC模式下的视图View一致。模板用来呈现Django view传来的数据，也决定了用户界面的外观。Template里面也包含了表单，可以用来搜集用户的输入内容。



Django工作流程：

当用户发来一个请求(request)时，Django会对请求头信息进行解析，解析出用户需要访问的url地址，然后根据路由urls.py中的定义的对应关系把请求转发到相应的视图处理。视图会从数据库读取需要的数据，指定渲染模板，最后返回响应数据

![](/post/Python/Django/1732094182588-90c28716-2202-46cf-adf8-e20294930f80.png)

简易开发流程

+ 新建app并注册

先使用`python manage.py startapp web`创建一个名为web的app，并将其添加到settings.py中

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'web.apps.WebConfig',
]
```

+ 在urls中添加路径与视图函数对应关系

![](/post/Python/Django/1732094938489-95a7b53c-5abc-42fa-a12a-0c1566f89138.png)

+ 创建模型

在app下新建名为models.py，在其中编写数据库字段及表与表的关联关系

![](/post/Python/Django/1732095241289-bb64416f-c9c6-4e57-b3a1-79e75148829f.png)

使用`python manage.py makemigrations`和`python manage.py migrate`命令让django在数据库中创建数据表

+ 创建视图函数

在app下新建名为views的目录，根据业务功能创建account.py，并在其中定义login函数，从数据库读取数据，处理逻辑，指定渲染模板并将数据传递给对应模板

![](/post/Python/Django/1732095370662-e9a615cc-c9d0-4b4c-98d5-464042123b53.png)

+ 编辑模板

创建在web/templates下创建login.html文件用于展示视图传来的数据，Django还提供了自己的模板语言用来渲染模板。

![](/post/Python/Django/1732095544263-c6c72fdd-a862-458a-9ca9-fa81008113c5.png)



## 路由配置
项目文件夹和每个应用(app)目录下的`urls.py`文件构成了Django的路由配置系统(URLconf)。服务器收到用户请求后，会根据用户请求的url地址和urls.py里配置的url-视图映射关系，去调用执行相应的视图函数或视图类，最后由视图返回给客户端数据

### URLconf如何工作
#### 手动路由分发
```python
from django.urls import path
from apps.web import views

urlpatterns = [
    path('user/add/', views.login),
	path('user/delete/', views.login),
	path('user/edit/', views.login),
	path('user/list/', views.login),
	
	# 提取公共部分，防止重复编写
	path('user/', ([
	                   path('add/', views.login),
	                   path('delete/', views.login),   # /user/delete/
	                   path('edit/', views.login),
	                   path('list/', views.login),
	               ], None, None)),
]
```

#### include+app，将功能拆分到不同的app中
要把app的urls加到项目的URL配置中

```python
from django.urls import include, path

urlpatterns = [
    path('web/', include('apps.web.urls')),
    ...
]
```

```python
from django.urls import include, path
from apps.api import views

urlpatterns = [
	# web/auth
    path('auth/', views.auth),
]
```

### URL设计方法
#### path`<变量类型:变量名>`

```python
urlpatterns = [
    path('news/<int:nid>/edit/', views.news),
]
```

+ int，整数
+ str，字符串
+ slug，字母+数字+下划线+-
+ uuid，uuid格式
+ path，路径，可以包含/

#### re_path`(?p<变量名>表达式)`
```python
urlpatterns = [
	re_path(r'user/(?P<xxid>\w+-\d+)/(?P<yid>\d+)/', views.user),
]
```

### 路由分发本质
URL对应函数

```python
path('user/add/', views.login),
```

URL对应元组

```python
path('user/',    (元素,appname元素,namespance元素)    ),
```

```python
path('user/',    include("apps.api.urls")    ),# include本质返回的就是一个形如([],None,None))的元组
path('user/',     ([],None,None)     ),
```

include源码解读

```python
def include(arg, namespace=None):
    app_name = None
    if isinstance(arg, tuple):
        try:
            urlconf_module, app_name = arg  # urlconf_module为列表，app_name为None
        except ValueError:
            ...
    else:
        urlconf_module = arg # urlconf_module被赋值为一个字符串如"apps.api.urls"
        
    if isinstance(urlconf_module, str):
        urlconf_module = import_module(urlconf_module) # urlconf_module为导入的模块如urls
    patterns = getattr(urlconf_module, 'urlpatterns', urlconf_module)  # 利用反射语法去取urls模块中的值，此处patterns为一个列表，当urlconf_module为列表时，patterns为默认值即列表
    app_name = getattr(urlconf_module, 'app_name', app_name)  # app_name为None
    if namespace and not app_name:
        raise ImproperlyConfigured(
            'Specifying a namespace in include() without providing an app_name '
            'is not supported. Set the app_name attribute in the included '
            'module, or pass a 2-tuple containing the list of patterns and '
            'app_name instead.',
        )
    namespace = namespace or app_name  # namespace=app_name=None

    if isinstance(patterns, (list, tuple)):
        for url_pattern in patterns:
            pattern = getattr(url_pattern, 'pattern', None)
            if isinstance(pattern, LocalePrefixPattern):
                raise ImproperlyConfigured(
                    'Using i18n_patterns in an included URLconf is not allowed.'
                )
    return (urlconf_module, app_name, namespace)  # ([],None,None)
```

```python
path('user/add/', views.login),
path('user/delete/', views.login),
path('user/edit/', views.login),
path('user/list/', views.login),


path('user/', ([
                   path('add/', views.login),
                   path('delete/', views.login),
                   path('edit/', views.login),
                   path('list/', views.login),
               ], None, None)),
               
               
path('users', include(([
                           path('add/', views.login),
                           path('delete/', views.login),
                           path('edit/', views.login),
                           path('list/', views.login),
                       ], None))),
                       
include("apps.api.urls")  # 一般是每个app中的urls，但其实是可以随意写的，只要其中有urlpatterns参数即可
urlpatterns = [

]
```

### name
相当于给每个URL取了个全局变量的名字。可以让你能够在Django的任意处，尤其是模板内显式地引用它

```python
urlpatterns = [
    path('Login/<str:role>/', views.login, name="v1"),
    re_path(r'auth/(\d+)/(\w+)/', views.auth, name="v2"),
    re_path(r'xxxx/(?P<nid>\d+)/(?P<tpl>\w+)/', views.auth, name="v3"),
]
```

+ 在视图函数中生成URL

> 有参数名传参用kargs={k1:k2}
>
> 无参数名传参用args=(v1,v2)
>

```python
from django.urls import reverse

url1 = reverse("v1", kwargs={"role": "hhh"})
url2 = reverse("v2", args=(666, "hhh"))
```

+ 在模板中生成URL

> `url`是个模板标签，其作用是对命名的url进行方向解析，动态生成链接。
>
> 命名的url里有几个参数，使用`url`模板标签反向生成动态链接时，就需要向它传递几个参数
>
> 有名字的要给他传值，无名字的空格分隔即可
>

```python
<a href="{% url 'v1' role="111" %}">跳转</a>
<a href="{% url 'v2' 666 "111" %}">跳转</a>
```

+ 拓展

```python
用name属性配合做权限管理
```

### namespace
假设不同的app（比如web和api）中有同名url该如何区分？

主路由

```python
from django.urls import path, re_path, include

# 很多功能，很多URL
urlpatterns = [
    path('api/', include("apps.api.urls",namespace='x1')),
    path('web/', include("apps.web.urls",namespace='x2')),
]
```

api/urls.py

```python
from django.urls import path, re_path
from . import views
# 很多功能，很多URL
urlpatterns = [
    path('login/', views.login,name="login"),
    path('auth/', views.auth, name='auth'),
]
app_name = "api"
```

web/urls.py

```python
from django.urls import path, re_path
from . import views
# 很多功能，很多URL
urlpatterns = [
    path('home/', views.home,name='home'),
    path('order/', views.order,name='order'),
    path('auth/', views.order, name='auth'),
]
app_name = "web"
```

只需要在`web/urls.py`加上`app_name='web'`这个命名空间即可

#### 视图中使用
```python
from django.urls import reverse

url = reverse("x1:auth")    # /api/auth/
url2 = reverse("x2:auth")    # /web/auth/
```

#### 模板中使用
```python
<a href="{% url 'x1:auth' %}">跳转</a>
<a href="{% url 'x2:auth' %}">跳转</a>
```

补充

![](/post/Python/Django/1732101851412-65b76dda-2530-4dce-a946-d35005525825.png)

![](/post/Python/Django/1732101903439-47168c0f-aa3d-41f4-8d00-bb46e599e925.png)



### url末尾的/
`APPEND_SLASH`是一个设置项，用于确定Django是否自动为没有尾随斜杠（`/`）的URL添加斜杠。默认情况下，`APPEND_SLASH`设置为`True`，这意味着如果用户请求的URL没有以斜杠结尾，Django会发出一个301重定向到带有尾随斜杠的URL



### 当前匹配对象
`ResolverMatch`对象是在URL解析过程中创建的，它包含了匹配到的URL模式的相关信息。当你访问一个URL时，Django的URL解析器会尝试找到匹配的模式，如果找到，它会创建一个`ResolverMatch`对象，并将其存储在请求对象的`resolver_match`属性中。

`ResolverMatch`对象在URL解析发生后才设置，这意味着它在所有视图中都是可用的，但在URL解析发生前的中间件中不可用（process_request）

```python
def my_view(request):
    resolver_match = request.resolver_match
```

作用

+ 直接访问URL中捕获的参数
+ 根据参数不同处理视图逻辑分支
+ URL名称解析，用来动态生成链接
+ 中间件支持，做权限控制

### partial
```python
def _xx(a1, a2):
    return a1 + a2


data = _xx(11, 22)
print(data)
```

```python
from functools import partial

def _xx(a1, a2):
    return a1 + a2

yy = partial(_xx, a2=100)

data = yy(2)
print(data)
```

![](/post/Python/Django/1732102693440-7bbb4b99-7711-46b6-a0d9-981da50359f6.png)

利用partial使得_path函数传入的Pattern不同，处理路由的方式不同

## 视图
### 文件or文件夹
所有视图函数放在一个view.py会难以维护且影响可读性，建议根据功能业务不同命名对应的py文件，将所有py文件放在views目录下

### 绝对导入和相对导入
原则：

能用绝对导入就用绝对导入，层级过深不方便再用相对导入

**一定不要在项目根目录做相对导入**

****

### 视图参数
```python
from django.shortcuts import HttpResponse


def login(request):
    return HttpResponse("login")
```

request是什么呢？

> request是由Django的中间件系统自动创建并传递给视图的一个对象，封装存放了浏览器发给服务器的所有内容，包括请求相关内容和django额外添加的数据
>



```python
from django.shortcuts import HttpResponse


def login(request):
    # 1.当前URL  /api/login/
    print(request.path_info)

    # 2.URL传递的参数
    print(request.GET)
    print(request.GET.get("age"))

    # 3.请求方式  GET/POST
    print(request.method)

    # 4.如果post请求，传递请求体（原始数据）
    print(request.body)  # b'{"code":"083Sjmll2yla694F3bll2DguCM2SjmlG","unionId":"oP6QCsyT_9bk1dfSaVf0GEV5Y-yE"}'  b'v1=123&v2=456'

    # 4.1 请求体+请求头       b'v1=123&v2=456'  +  content-type:application/x-www-form-urlencoded
    print(request.POST)
    print(request.POST.get("v1"))
    print(request.POST.get("v2"))

    # 4.2 请求体+请求头   文件
    print(request.FILES)  # 文件格式           + content-type:multipart/form-data
    print(request.FILES.get("n1"))
    print(request.FILES.get("n2"))

    # 5.请求头
    # {'Content-Length': '', 'Content-Type': 'text/plain', 'Host': '127.0.0.1:8000', 'Connection': 'keep-alive', 'Cache-Control': 'max-age=0', 'Sec-Ch-Ua': '" Not A;Brand";v="99", "Chromium";v="102", "Google Chrome";v="102"', 'Sec-Ch-Ua-Mobile': '?0', 'Sec-Ch-Ua-Platform': '"macOS"', 'Upgrade-Insecure-Requests': '1', 'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Safari/537.36', 'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9', 'Sec-Fetch-Site': 'none', 'Sec-Fetch-Mode': 'navigate', 'Sec-Fetch-User': '?1', 'Sec-Fetch-Dest': 'document', 'Accept-Encoding': 'gzip, deflate, br', 'Accept-Language': 'zh-CN,zh;q=0.9,en;q=0.8,zh-TW;q=0.7', 'Cookie': 'csrftoken=CdidpKSGbLxzmOXnbmlkvrZep1eJmKLAA81T73UjcjxEnMOa4YOZqtc849AkYfUy'}
    print(request.headers)

    # 5.1 请求头有个特殊的cookie
    # request.headers['cookie']  # 'csrftoken=CdidpKSGbLxzmOXnbmlkvrZep1eJmKLAA81T73UjcjxEnMOa4YOZqtc849AkYfUy;session=xxxx'
    # {'csrftoken': 'CdidpKSGbLxzmOXnbmlkvrZep1eJmKLAA81T73UjcjxEnMOa4YOZqtc849AkYfUy'}
    print(request.COOKIES)

    # 6.requests中其他值
    print(request.resolver_match)

    return HttpResponse("login")
```



补充：request源码解读

```python
def _get_post(self):
    # 去请求体解析body赋值给_post
    if not hasattr(self, '_post'):
        self._load_post_and_files()
    return self._post

def _set_post(self, post):
    self._post = post
    
@property
def FILES(self):
    # 去请求体解析body赋值给_files
    if not hasattr(self, '_files'):
        self._load_post_and_files()
    return self._files

POST = property(_get_post, _set_post)  # 查询时执行_get_post，赋值时执行_set_post
```

_load_post_and_files()函数解析

```python
def _load_post_and_files(self):
    """Populate self._post and self._files if the content-type is a form type"""
    if self.method != 'POST':
        self._post, self._files = QueryDict(encoding=self._encoding), MultiValueDict()
        return
    if self._read_started and not hasattr(self, '_body'):
        self._mark_post_parse_error()
        return

    if self.content_type == 'multipart/form-data':
        if hasattr(self, '_body'):
            # Use already read data
            data = BytesIO(self._body)
        else:
            data = self
        try:
            self._post, self._files = self.parse_file_upload(self.META, data)
        except MultiPartParserError:
            # An error occurred while parsing POST data. Since when
            # formatting the error the request handler might access
            # self.POST, set self._post and self._file to prevent
            # attempts to parse POST data again.
            self._mark_post_parse_error()
            raise
    elif self.content_type == 'application/x-www-form-urlencoded':
        self._post, self._files = QueryDict(self.body, encoding=self._encoding), MultiValueDict()
    else:
        self._post, self._files = QueryDict(encoding=self._encoding), MultiValueDict()
```

### 返回值
+ HttpResponse
+ JsonResponse
+ render
+ redirect

```python
# 3.1 字符串/字节/文本数据（图片验证码）
return HttpResponse("login")

# 3.2 JSON格式（前后端分离、app小程序后端、ajax请求）
data_dict = {"status": True, 'data': [11, 22, 33]}
return JsonResponse(data_dict)

# 3.3 重定向
return redirect("https://www.baidu.com")
return redirect("http://127.0.0.1:8000/api/auth/")
return redirect("/api/auth/")  # 省略http://127.0.0.1:8000

return redirect("auth")  # name
from django.urls import reverse
url = reverse("auth")
return redirect(url)  # name


# 3.4 渲染
# - a.找到 'login.html' 并读取的内容，问题：去哪里找？
# -   默认先去settings.TEMPLATES.DIRS指定的路径找。（公共）
# -   按注册顺序每个已注册的app中找他templates目录，去这个目录中寻找'login.html'
# -   一般情况下，原则，那个app中的的模板，去哪个那个app中寻找。
# - b.渲染（替换）得到替换完成的字符串
# - c.返回浏览器
return render(request, 'api/login.html')
```

html文件放置

![](/post/Python/Django/1732103923915-1c02e27c-d9b9-4a1a-94da-f87b6d04111c.png)



### 响应头
```python
from django.shortcuts import HttpResponse, redirect, render
from django.http import JsonResponse


def login(request):
    # 设置响应内容
    res = HttpResponse("login")
    # 设置响应头
    res['xx1'] = "hahaha"
    res['xx2'] = "hahaha"
    res['xx3'] = "hahaha"
	# 设置cookie
    res.set_cookie('k1',"aaaaaaaa")
    res.set_cookie('k2',"bbbbbb")

    return res
```

### FBV和CBV
![](/post/Python/Django/1732104063621-29bab11d-9779-48b7-bd77-8643fc50c51a.png)

源码分析

```python
# 自定义的UsersView类没有as_view()方法，于是找其父类View的as_view()方法
def as_view(cls, **initkwargs):
    """Main entry point for a request-response process."""
    for key in initkwargs:
        ...
    def view(request, *args, **kwargs):
        # self=UsersView()
        self = cls(**initkwargs)
        self.setup(request, *args, **kwargs)
        if not hasattr(self, 'request'):
            ...
        return self.dispatch(request, *args, **kwargs)
    ...
    return view
 def dispatch(self, request, *args, **kwargs):
        if request.method.lower() in self.http_method_names:
            # hander为UsersView对象的get方法或post方法
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
        else:
            handler = self.http_method_not_allowed
        # hander() 调用
        return handler(request, *args, **kwargs)
# 本质还是一个函数
```

## 静态资源
+ 开发需要：css，js，图片

```python
- 根目录的 /static/
- 已经app目录下载 /static/ 文件夹下
```

+ 媒体文件：用户上传的数据

```python
- 根目录的 /media/
```

### 静态文件
```python
# 已注册的app的static目录
INSTALLED_APPS = [
    # 'django.contrib.admin',
    # 'django.contrib.auth',
    # 'django.contrib.contenttypes',
    # 'django.contrib.sessions',
    # 'django.contrib.messages',
    'django.contrib.staticfiles',
    "apps.api.apps.ApiConfig",
    "apps.web.apps.WebConfig",
]
...
# 根目录下的static目录
STATIC_URL = '/static/'
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'static'), # 项目级别的静态文件目录
)
```

顺序：优先查找根目录下的static目录，若找不到再按注册顺序去已注册的app下的static目录

如何导入？

导入模块，调用里面的函数帮我去setting文件中读取STATIC_URL

```python
{% load static %}

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>登录页面</h1>
<a href="/xxx/xxxxx/">调换dao xx</a>
<a href="{% url 'login' %}">跳转</a>

<img src="{% static 'api/1.png' %}">

</body>
</html>
```

生产环境

+ 收集静态文件

```python
# 将所有的静态文件收集到一个位置，以便Web服务器可以高效地提供这些文件
python manage.py collectstatic
```

+ 配置Web服务器

在生产环境中，你需要配置Web服务器（如Nginx或Apache）来提供静态文件。你需要指定静态文件的目录（通常是 `STATIC_ROOT`）

### 媒体文件
在你的 `settings.py` 文件中，你需要定义两个设置项：

+ `MEDIA_URL`：媒体文件的URL前缀。
+ `MEDIA_ROOT`：服务器上存储上传文件的路径。

```python
# settings.py

# 上传的文件可以通过类似 http://yourdomain.com/media/yourfile.jpg 的URL来访问。
MEDIA_URL = '/media/'
# BASE_DIR 是你的Django项目的根目录，media 是在项目根目录下的一个子目录，用于存储上传的文件
MEDIA_ROOT = BASE_DIR / 'media'
```

在 `urls.py` 中添加以下URL模式，在开发环境中运行服务器时，Django将能够通过 `/media/` URL前缀提供上传的文件

```python
# urls.py

from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # ... 你的其他url配置
]

if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

## 模板
### html模板的寻找顺序
```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')], 
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                # 'django.contrib.auth.context_processors.auth',
                # 'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

参数介绍

| BACKEND  | 指定模板引擎的后端                                           |
| -------- | ------------------------------------------------------------ |
| DIRS     | 一个包含额外模板目录路径的列表,通常，这里用于存放全局模板    |
| APP_DIRS | 如果设置为`True`，则Django会在每个安装的应用的`templates`目录下寻找模板 |
| OPTIONS  | 一个字典，包含模板引擎的选项                                 |


寻找顺序

+ 如果配置了DIRS目录，则Django会现在这些目录中寻找
+ 在DIRS中找不到，会继续在每个app的templates目录中搜索，且顺序按照INSTALLED_APP的注册顺序进行



如何选择？

+ 简单的项目，模板都放在根目录
+ 复杂的项目，模板放在各自的app中，公共部分放在templates目录



拓展：修改内置app的模板

> 在根目录的templates目录下创建同名文件夹同名html文件，即可覆盖内置app提供的模板，因为有优先查找DIRS目录
>



### 模板处理的本质
渲染完成后，生成了字符串，再返回给浏览器

> 文件命名为.html只是为了编写html方便，实际上什么后缀都可以
>

模板处理流程

+ 视图函数获取数据，创建上下文字典并传递给模板

```python
from django.shortcuts import render
from .models import MyModel

def home(request):
    data = MyModel.objects.all()  # 从数据库获取数据
    context = {'data': data}  # 创建上下文
    return render(request, 'myapp/home.html', context)  # 渲染模板
```

+ 渲染模板 render(）函数
    - 根据模板名称查找模板
    - 加载模板，准备渲染
+ 处理上下文
    - 将上下文字典插入模板。模板中的占位符（变量和标签）会被实际数据替代
    - 处理模板中的模板标签和过滤器，例如：循环遍历数据、条件判断等

```python
<h1>My Data</h1>
<ul>
    {% for item in data %}
        <li>{{ item.name }}</li>
    {% endfor %}
</ul>
```

+ 生成最终输出，返回响应



![](/post/Python/Django/1732153383490-62011243-3759-49fc-b4ce-7a2542acc514.png)

弹窗内容为{{ n2 }}，因为在js文件是在浏览器端发起的请求，此时模板渲染早已完成



### 常用语法
变量传递

> 调用方法不用加（）
>

![](/post/Python/Django/1732154380277-df3b82d6-2172-4972-a56c-cc37bbb63e96.png)

内置函数

![](/post/Python/Django/1732154498692-89b2f800-e9b8-48f2-b443-2815d348f102.png)

标签

```python
# if
{% if condition %}
    <!-- code -->
{% endif %}

# for
{% for item in items %}
    <!-- code -->
{% endfor %}

# url
{% url 'name' arg %}
```

模板继承

```python
{% extends "base.html" %}

{% block title %}{% endblock %}
{% block content %}{% endblock %}
```

自定义模板标签

+ filter

> 用于修改变量值，通常在模板变量后面使用，用管道符号`|`分隔
>

```python
- 数据处理，参数：1-2个
- 数据处理，if条件
```

```python
# 在templatetags/custom_filters.py中
from django import template

register = template.Library()

@register.filter(name='lowercase')
def lowercase(value):
    return value.lower()
```

模板中

```html
{{ my_variable|lowercase }}
```

+ simple_tag

> 一个自定义的模板标签，它不接受任何模板变量，只接收位置参数和关键字参数，并返回一个字符串结果
>

```python
参数无限制 & 返回文本
```

```python
# 在templatetags/custom_tags.py中
from django import template

register = template.Library()

@register.simple_tag
def my_simple_tag(arg):
    return f"<p>{arg}</p>"
```

模板中

```html
{% my_simple_tag "Hello, World!" %}
```

+ inclusion_tag

> 类似于simple_tag，但它返回的是渲染后的模板片段，而不是一个简单的字符串。这允许你将模板的某个部分抽象成一个独立的模板文件，并在需要时包含进来
>

```python
参数无限制，返回html片段
```

```python
# 在templatetags/custom_tags.py中
from django import template

register = template.Library()

@register.inclusion_tag('custom_template.html')
def my_inclusion_tag(data):
    return {'data': data}
```

custom_template.html文件

```html
<div>
	<p>{{ data }}</p>
</div>
```

模板中

```python
{% my_inclusion_tag "Some data" %}
```

### 继承与母版


#### 基础模板（母版）
通常包含以下内容

+ 通用的HTML结构
+ 占位符（block)，用于字幕版覆盖或扩展内容

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}My Site{% endblock %}</title>
</head>
<body>
    <header>
        <!-- 通用头部内容 -->
    </header>
    
    {% block content %}
    <!-- 默认内容，子模板可以覆盖或扩展 -->
    {% endblock %}
    
    <footer>
        <!-- 通用底部内容 -->
    </footer>
</body>
</html>
```

#### 子模板
子模板继承基础模板，并可以覆盖或扩展其中的内容

```html
{% extends "base.html" %}

{% block title %}Home Page{% endblock %}

{% block content %}
    <h1>Welcome to the Home Page</h1>
    <p>This is the home page content.</p>
{% endblock %}
```

母版一般放在根目录下的templates目录，若要视app为组件，可在app下的templates也放一份



#### 模板的导入
![](/post/Python/Django/1732156134550-7ae71e20-74ab-4cf3-bfbf-b308bdc597bc.png)

先继承模板，再导入嵌套模板，最后再进行渲染

```html
{% include "template_name.html" %}
```

## 中间件
Django生命周期

![](/post/Python/Django/1732156310706-5f8a5e35-07be-4e03-9b5c-f8cc8a437e41.png)

+ 类
+ 定义方法
+ 注册

### 简单实例（原始）
![](/post/Python/Django/1732156549515-a6a6761d-fe06-4d55-a6bb-bed26c8f59fa.png)

![](/post/Python/Django/1732156555542-dee6a05a-5ace-48ed-a4c3-83c7c5aa76c3.png)

### MiddlewareMixin（推荐）
#### 食用方法
自定义process_request和process_response方法，其他继承MiddlewareMixin即可

![](/post/Python/Django/1732156660779-a5e2accc-5f34-4743-b16e-1de4162bbcd8.png)

![](/post/Python/Django/1732156674883-057ef91a-1730-41ff-9973-04353a8d72b1.png)



源码

```python
class MiddlewareMixin:
	def __init__(self, get_response=None):
		self.get_response = get_response

	def __call__(self, request):
		response = None
		if hasattr(self, 'process_request'):
			response = self.process_request(request)
		response = response or self.get_response(request)
		if hasattr(self, 'process_response'):
			response = self.process_response(request, response)
		return response


class MyMd(MiddlewareMixin):

	def process_request(self,request):
		...

	def process_response(self,request, response):
		...

django内部默认执行call方法，传入参数。
```

#### 执行流程
![](/post/Python/Django/1732156863476-dd0017c5-a2b6-42b0-97b3-ef8bbf4c0bee.png)

![](/post/Python/Django/1732156875903-033f2390-2324-440e-b3a1-119536220a07.png)

> process_request都没有返回值，如果有返回值则直接执行其process_response，不再执行后续中间件及视图函数
>
> 用于权限判断，ip黑名单拦截
>
> process_response都有返回值，用于给response添加信息
>

![](/post/Python/Django/1732156982789-92493c72-3b89-47e8-b900-5abb53163ef2.png)

中间件间的执行顺序按照注册顺序



总结：

先按顺序依次执行各个中间件的process_request，再进行路由匹配，在返回去执行process_view方法才进入视图函数，若某一process_view有返回值，则直接跳走，完整执行process_reponse

> process_view是在django中写死了的
>

![](/post/Python/Django/1732157126083-056bdf52-84e4-43c5-a1ef-de12c1307d1c.png)



#### 其他
##### process_exception
> 当视图抛出异常时，Django会寻找一个合适的响应
>
> Django会从后向前遍历中间件栈，调用每个中间件的`process_exception`方法，直到找到一个返回了响应对象（`HttpResponse`或其子类实例）的方法，如果到达中间件栈的开始都没有找到返回响应的方法，Django将显示默认的500错误页面（服务器内部错误）
>
> 这个方法允许开发者在异常发生时执行一些清理工作，或者返回一个自定义的响应给客户端，而不是默认的错误页面
>

![](/post/Python/Django/1732157307696-c14048f3-9e5d-46f3-920e-936258e8efe0.png)

![](/post/Python/Django/1732157312628-1d652b10-e89d-47b3-9e86-33d5c9c83a3d.png)



##### process_template_response
> 在视图被完全执行后调用，如果响应实例有`render()`方法，表明它是一个`TemplateResponse`或等效对象。这个方法允许你在模板渲染之后、响应发送到客户端之前，对`TemplateResponse`对象进行处理
>

执行顺序

+ `process_template_response`方法在视图函数返回一个`TemplateResponse`对象后被调用。
+ 如果视图返回的是一个`TemplateResponse`对象，Django会从中间件栈的末尾开始调用中间件的`process_template_response`方法，直到找到一个返回了`TemplateResponse`对象的方法或者到达中间件栈的开始。
+ 中间件在响应阶段会按照相反的顺序运行，其中包括`process_template_response`。



#### 总结
+ 定义中间类
+ 类方法
    - process_request
    - process_view
    - process_reponse
    - process_exception，视图函数出现异常，自定义异常页面。
    - process_template_response，视图函数返回TemplateResponse对象  or  对象中含有.render方法。



## ORM操作
ORM（Object-Relational Mapping，对象关系映射）是Django框架的核心组件之一，它允许开发者使用Python代码来操作数据库，而无需编写SQL语句。ORM抽象了数据库的操作，使得开发者可以像操作普通Python对象一样操作数据库

**本质就是翻译的**

![](/post/Python/Django/1732162192972-fdc2e43c-6249-4700-87ce-05447e6c372a.png)

特点：开发效率高，执行效率低

### ORM基本操作步骤
+ settings.py，连接数据库

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

+ settings.py，注册app

```python
INSTALLED_APP = [
	...
	"app01.apps.App01Config"
]
```

+ 编写models类

```python
class UserInfo(models.Model):
    ....
    .....
```

+ 执行命令

```python
python manage.py makemigrations    # 创建迁移文件，找到所有已注册的app中的models.py中的类读取 -> migrations配置
python manage.py migrate           # 应用迁移到数据库，读取已注册的app下的migrations配置 -> SQL语句  -> 同步数据库
# 自带的app已默认生成migrations
```

### 连接数据库
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'xxxxxxxx',  # 数据库名字
        'USER': 'root',
        'PASSWORD': 'root123',
        'HOST': '127.0.0.1',  # ip
        'PORT': 3306,
    }
}
```

#### 项目连接MySQL：
+ 安装MySQL并启动MySQL服务
+ 手动创建数据库
+ 配置django的settings.py

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'xxxxxxxx',  # 数据库名字
        'USER': 'root',
        'PASSWORD': 'root123',
        'HOST': '127.0.0.1',  # ip
        'PORT': 3306,
    }
}
```

+ 安装第三方组件 选其一即可
    - pymysql

```python
pip install pymysql
```

```python
项目根目录/项目名目录/__init__.py
	import pymysql
	pymysql.install_as_MySQLdb()
```

    - mysqlclient

```python
pip install mysqlclient
```

#### 其他数据库
##### pgsql
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'mydatabase',
        'USER': 'mydatabaseuser',
        'PASSWORD': 'mypassword',
        'HOST': '127.0.0.1',
        'PORT': 5432,
    }
}

# 需要 pip install psycopg2
```

```python
DATABASES = {
	'default': {
        'ENGINE': 'django.db.backends.oracle',
        'NAME': "xxxx",  # 库名
        "USER": "xxxxx",  # 用户名
        "PASSWORD": "xxxxx",  # 密码
        "HOST": "127.0.0.1",  # ip
        "PORT": 1521,  # 端口
    }
}
# 需要 pip install cx-Oracle
```

### 连接池

django默认内置没有连接池

```python
pymysql -> 操作数据库
DBUtils -> 连接池
```

django-db-connection-pool组件

```python
pip install django-db-connection-pool
```

```python
DATABASES = {
    "default": {
        'ENGINE': 'dj_db_conn_pool.backends.mysql',
        'NAME': 'day04',  # 数据库名字
        'USER': 'root',
        'PASSWORD': 'root123',
        'HOST': '127.0.0.1',  # ip
        'PORT': 3306,
        'POOL_OPTIONS': {
            'POOL_SIZE': 10,  # 最小
            'MAX_OVERFLOW': 10,  # 在最小的基础上，还可以增加10个，即：最大20个。
            'RECYCLE': 24 * 60 * 60,  # 连接可以被重复用多久，超过会重新创建，-1表示永久。
            'TIMEOUT':30, # 池中没有连接最多等待的时间。
        }
    }
}
```



### 多数据库
django支持项目连接多个数据库

```python
DATABASES = {
    "default": {
        'ENGINE': 'dj_db_conn_pool.backends.mysql',
        'NAME': 'day05db',  # 数据库名字
        'USER': 'root',
        'PASSWORD': 'root123',
        'HOST': '127.0.0.1',  # ip
        'PORT': 3306,
        'POOL_OPTIONS': {
            'POOL_SIZE': 10,  # 最小
            'MAX_OVERFLOW': 10,  # 在最小的基础上，还可以增加10个，即：最大20个。
            'RECYCLE': 24 * 60 * 60,  # 连接可以被重复用多久，超过会重新创建，-1表示永久。
            'TIMEOUT': 30,  # 池中没有连接最多等待的时间。
        }
    },
    "bak": {
        'ENGINE': 'dj_db_conn_pool.backends.mysql',
        'NAME': 'day05bak',  # 数据库名字
        'USER': 'root',
        'PASSWORD': 'root123',
        'HOST': '127.0.0.1',  # ip
        'PORT': 3306,
        'POOL_OPTIONS': {
            'POOL_SIZE': 10,  # 最小
            'MAX_OVERFLOW': 10,  # 在最小的基础上，还可以增加10个，即：最大20个。
            'RECYCLE': 24 * 60 * 60,  # 连接可以被重复用多久，超过会重新创建，-1表示永久。
            'TIMEOUT': 30,  # 池中没有连接最多等待的时间。
        }
    },
}
```

#### 读写分离
```python
192.168.1.2       default master   [写]
                  组件
192.168.2.12      bak slave    [读]
```

+ 生成数据库表

```python
python manage.py makemigrations    # 找到所有已注册的app中的models.py中的类读取 -> migrations配置

python manage.py migrate
python manage.py migrate --database=default
python manage.py migrate --database=bak
```

+ 后续再进行开发时

```python
models.UserInfo.objects.using("default").create(title="武沛齐")

models.UserInfo.objects.using("bak").all()
```

+ 编写router类，简化后续开发时操作

```python
class DemoRouter(object):
    
    def db_for_read(...):
        return "bak"
    
    def db_for_write(...):
        return "default"
```

```python
router = ["DemoRouter"]
```



```python
- settings文件配置 DATABASE_ROUTERS
- 编写settings中配置的文件和类
进行数据库操作时，会根据读还是写进入db_for_read或db_for_write函数，然后根据其返回的数据库名对其进行操作
```



![](/post/Python/Django/1732325909611-81b8f2b8-b1d8-4f6f-93d1-55edf06e2e2c.png)



#### 分库（多app -> 多数据库）
100张表，A库存放app01生成的50张表，B库存放app02生成的50张表

+ app01/models

```python
from django.db import models


class UserInfo(models.Model):
    title = models.CharField(verbose_name="标题", max_length=32)
```

+ app02/models

```python
from django.db import models


class Role(models.Model):
    title = models.CharField(verbose_name="标题", max_length=32)
```

+ 命令

```python
python manage.py makemigrations
python manage.py migrate app01 --database=default
python manage.py migrate app02 --database=bak
```

![](/post/Python/Django/1732326481315-96d08a97-ae7f-49ca-8895-832b28cde414.png)

+ 读写操作

```python
from django.shortcuts import render, HttpResponse

from app01 import models as m1
from app02 import models as m2


def index(request):
    # app01中的操作 -> default
    v1 = m1.UserInfo.objects.all()
    print(v1)

    # app02中的操作 -> bak
    v2 = m2.Role.objects.using('bak').all()
    print(v2)
    return HttpResponse("返回")
```

+ router

![](/post/Python/Django/1732326552654-4d53afce-ca65-4eda-9bf8-0a7c80cf3798.png)



#### 分库（单app）
100张表，20张表-A数据库；50张表-B数据库

![](/post/Python/Django/1732326718970-ad2e5342-406c-48a9-b91f-44e58e3cc8e0.png)

```python
from django.shortcuts import render, HttpResponse

from app01 import models as m1


def index(request):
    # app01中的操作 -> default
    v1 = m1.UserInfo.objects.all()
    print(v1)

    # app01中的操作 -> bak
    v2 = m1.Role.objects.using('bak').all()
    print(v2)

    return HttpResponse("返回")
```

![](/post/Python/Django/1732327229566-09bc94b8-3a1d-4c39-8bc5-66660b0832ed.png)



#### 注意事项
+ 分库，将表拆分到不同的数据库

```python
不要做跨数据库关联 -> django不支持

尽可能将关联的表放到一个库中
```

+ 为什么分库

减轻数据库压力



### 表关系
+ 单表

```python
class Role(models.Model):
    title = models.CharField(verbose_name="标题", max_length=32)
```

+ 一对多

```python
class Meta:
	db_table = "depart" # 自定义表名，默认是app名_表名
	index_together={} # 联合索引
    unique_together=((),) # 联合唯一索引
```

![](/post/Python/Django/1732327662770-2fd0b964-e1aa-4fe9-b26c-035103380938.png)

+ 多对多

![](/post/Python/Django/1732327680736-3c72a242-eeb6-4b65-9bf6-763542287727.png)

如果关系只有3列

```python
class Boy(models.Model):
    """
    1   杰森斯坦森
    2   汤普森
    """
    name = models.CharField(verbose_name="标题", max_length=32, unique=True)
    b = models.ManyToManyField(to="Girl")

class Girl(models.Model):
    """
    1   alex
    2   苑昊
    """
    name = models.CharField(verbose_name="标题", max_length=32, unique=True)
```

```python
class Boy(models.Model):
    """
    1   杰森斯坦森
    2   汤普森
    """
    name = models.CharField(verbose_name="标题", max_length=32, unique=True)
    
class Girl(models.Model):
    """
    1   alex
    2   苑昊
    """
    name = models.CharField(verbose_name="标题", max_length=32, unique=True)
    b = models.ManyToManyField(to="Boy")
```

```python
# 初学阶段建议
class Boy(models.Model):
    name = models.CharField(verbose_name="标题", max_length=32, unique=True)


class Girl(models.Model):
    name = models.CharField(verbose_name="标题", max_length=32, unique=True)


class B2G(models.Model):
    bid = models.ForeignKey(to="Boy", on_delete=models.CASCADE)
    gid = models.ForeignKey(to="Girl", on_delete=models.CASCADE)
    address = models.CharField(verbose_name="地点", max_length=32)
```

+ 一对一

![](/post/Python/Django/1732327864468-81318d94-11f6-4107-bda8-01b951a84d53.png)

### 数据操作
#### 单表
+ 增

```python
class Role(models.Model):
    title = models.CharField(verbose_name="标题", max_length=32)
```

```python
obj1 = models.Role.objects.create(title="管理员", od=1)
obj2 = models.Role.objects.create(**{"title": "管理员", "od": 1})

内存 -> save
obj = models.Role(title="客户", od=1)
obj.od = 100
obj.save()

obj = models.Role(**{"title": "管理员", "od": 1})
obj.od = 100
obj.save()
```

控制输出格式 重写__str__方法

![](/post/Python/Django/1732328790674-7e46b45b-8bc0-4323-83cd-7c80b33db501.png)

+ 删

```python
# models.Role.objects.all().delete()
models.Role.objects.filter(title="管理员").delete()
```

+ 改

```python
models.Role.objects.all().update(od=99)
models.Role.objects.filter(id=7).update(od=99, title="管理员")
models.Role.objects.filter(id=7).update(**{"od": 99, "title": "管理员"})
```

+ 查

条件筛选

```python
# QuerySet = [obj, obj]
v1 = models.Role.objects.all()
for obj in v1:
    print(obj, obj.id, obj.title, obj.od)

# QuerySet = []
# v2 = models.Role.objects.filter(od=99, id=99)
v2 = models.Role.objects.filter(**{"od": 99, "id": 99})
for obj in v2:
    print(obj, obj.id, obj.title, obj.od)
    

v3 = models.Role.objects.filter(id=99)
print(v3.query)

v3 = models.Role.objects.filter(id__gt=2)
print(v3.query)  # >

v3 = models.Role.objects.filter(id__gte=2)
print(v3.query)  # >=

v3 = models.Role.objects.filter(id__lt=2)
print(v3.query)  # <

v3 = models.Role.objects.filter(id__in=[11, 22, 33])
print(v3.query)

v3 = models.Role.objects.filter(title__contains="户")
print(v3.query)

v3 = models.Role.objects.filter(title__startswith="户")
print(v3.query)

v3 = models.Role.objects.filter(title__isnull=True)
print(v3.query)
```

不等于

```python
v3 = models.Role.objects.filter(id=99)
print(v3.query)
# 不等于
v3 = models.Role.objects.exclude(id=99).filter(od=88)
print(v3.query)
```

获取字典，元组列表

```python
# queryset=[obj,obj]
v3 = models.Role.objects.filter(id=99)

# queryset=[{'id': 6, 'title': '客户'}, {'id': 7, 'title': '客户'}]
v4 = models.Role.objects.filter(id__gt=0).values("id", 'title')

# QuerySet = [(6, '客户'), (7, '客户')]
v5 = models.Role.objects.filter(id__gt=0).values_list("id", 'title')
print(v5[0])
```

获取单个对象

```python
v6 = models.Role.objects.filter(id__gt=0).first()
# print(v6)  # 对象

v7 = models.Role.objects.filter(id__gt=10).exists()
print(v7)  # True/False
```

排序

```python
# asc
v8 = models.Role.objects.filter(id__gt=0).order_by("id")

# id desc  od asc
v9 = models.Role.objects.filter(id__gt=0).order_by("-id", 'od')
```

#### 一对多
```python
class Depart(models.Model):
    """ 部门 """
    title = models.CharField(verbose_name="标题", max_length=32)


class Admin(models.Model):
    name = models.CharField(verbose_name="姓名", max_length=32)
    pwd = models.CharField(verbose_name="密码", max_length=32)

    depart = models.ForeignKey(verbose_name="部门", to="Depart", on_delete=models.CASCADE)
```

+ 增

```python
models.Admin.objects.create(name='武沛齐1', pwd='123123123', depart_id=2)
# models.Admin.objects.create(**{..})

obj = models.Depart.objects.filter(id=2).first()
models.Admin.objects.create(name='武沛齐2', pwd='123123123', depart=obj)
models.Admin.objects.create(name='武沛齐2', pwd='123123123', depart_id=obj.id)
```

+ 删

```python
# filter()   # 当前表的字段 + depart__字段    -> 连表和条件

# 找到部门id=3的所有的员工，删除
models.Admin.objects.filter(depart_id=3).delete()

# 删除销售部的所有员工
obj = models.Depart.objects.filter(title="销售部").first()
models.Admin.objects.filter(depart_id=obj.id).delete()
# 跨表
models.Admin.objects.filter(depart__title="销售部", name='武沛齐').delete()
```

+ 改

```python
# 查询
models.Admin.objects.filter(id=2).update(name='xxx', pwd='xxxx')
models.Admin.objects.filter(name="武沛齐").update(depart_id=2)

# models.Admin.objects.filter(id=2).update(depart__title="技术部")  -> 只能更新自己表字段
```

+ 查

```python
# 1. select * from admin    					queryset=[obj,obj,]
v1 = models.Admin.objects.filter(id__gt=0)
for obj in v1:
    print(obj.name, obj.pwd, obj.id, obj.depart_id)  # 以这种方式查找尽量不要跨表，效率很低

# 2. select * from admin inner join depart      queryset=[obj,obj,]
v2 = models.Admin.objects.filter(id__gt=0).select_related("depart")
for obj in v2: 
    print(obj.name, obj.pwd, obj.id, obj.depart_id, obj.depart.title)

# 3. select id,name.. from admin inner join depart      queryset=[{},{}]
v3 = models.Admin.objects.filter(id__gt=0).values("id", 'name', 'pwd', "depart__title")
print(v3)

# 4. select id,name.. from admin inner join depart      queryset=[(),()]
v4 = models.Admin.objects.filter(id__gt=0).values_list("id", 'name', 'pwd', "depart__title")
print(v4)
```

#### 多对多
```python
from django.db import models


class Boy(models.Model):
    name = models.CharField(verbose_name="姓名", max_length=32, db_index=True)


class Girl(models.Model):
    name = models.CharField(verbose_name="姓名", max_length=32, db_index=True)


class B2G(models.Model):
    bid = models.ForeignKey(to="Boy", on_delete=models.CASCADE)
    gid = models.ForeignKey(to="Girl", on_delete=models.CASCADE)
    address = models.CharField(verbose_name="地点", max_length=32)
```

```python
def index(request):
    models.Boy.objects.create(name="宝强")
    models.Boy.objects.create(name="羽凡")
    models.Boy.objects.create(name="乃亮")
    
    models.Girl.objects.bulk_create(
        objs=[models.Girl(name="小路"), models.Girl(name="百合"), models.Girl(name="马蓉")],
        batch_size=3
    )

    # 创建关系
    models.B2G.objects.create(bid_id=1, gid_id=3, address="北京")
    models.B2G.objects.create(bid_id=1, gid_id=2, address="北京")
    models.B2G.objects.create(bid_id=2, gid_id=2, address="北京")
    models.B2G.objects.create(bid_id=2, gid_id=1, address="北京")

    b_obj = models.Boy.objects.filter(name='宝强').first()
    g_object = models.Girl.objects.filter(name="小路").first()
    models.B2G.objects.create(bid=b_obj, gid=g_object, address="北京")

    # 1.宝强都与谁约会。
    queyset=[obj,obj,obj]
    q = models.B2G.objects.filter(bid__name='宝强').select_related("gid")
    for item in q:
        print(item.id, item.address, item.bid.name, item.gid.name)

    q = models.B2G.objects.filter(bid__name='宝强').values("id", 'bid__name', 'gid__name')
    for item in q:
        print(item['id'], item['bid__name'], item['gid__name'])

    # 2.百合 都与谁约会。
    q = models.B2G.objects.filter(gid__name='百合').values("id", 'bid__name', 'gid__name')
    for item in q:
        print(item['id'], item['bid__name'], item['gid__name'])

    # 3.删除
    models.B2G.objects.filter(id=1).delete()
    models.Boy.objects.filter(id=1).delete()

    return HttpResponse("返回")
```

#### 一对一
![](/post/Python/Django/1732336942184-224968b7-56b0-4464-9ba3-71fc23969d44.png)



## cookie和seeesion
### cookie
![](/post/Python/Django/1732337342278-0d558d6d-b66c-4fe7-8a19-db307b1287a8.png)

```python
127.0.0.1       v1.wupeiqi.com
127.0.0.1       v2.wupeiqi.com
```

![](/post/Python/Django/1732337452694-3063ebca-e902-4e52-8996-67df227dbcd7.png)

### 配置session
+ 文件版

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    # 'django.contrib.auth.middleware.AuthenticationMiddleware',
    # 'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]


# session
SESSION_ENGINE = 'django.contrib.sessions.backends.file'
SESSION_FILE_PATH = 'xxxx' 

SESSION_COOKIE_NAME = "sid"  # Session的cookie保存在浏览器上时的key，即：sessionid＝随机字符串
SESSION_COOKIE_PATH = "/"  # Session的cookie保存的路径
SESSION_COOKIE_DOMAIN = None  # Session的cookie保存的域名
SESSION_COOKIE_SECURE = False  # 是否Https传输cookie
SESSION_COOKIE_HTTPONLY = True  # 是否Session的cookie只支持http传输
SESSION_COOKIE_AGE = 1209600  # Session的cookie失效日期（2周）

SESSION_EXPIRE_AT_BROWSER_CLOSE = False  # 是否关闭浏览器使得Session过期
SESSION_SAVE_EVERY_REQUEST = True  # 是否每次请求都保存Session，默认修改之后才保存
```

+ 数据库版

```python
INSTALLED_APPS = [
    # 'django.contrib.admin',
    # 'django.contrib.auth',
    # 'django.contrib.contenttypes',
    'django.contrib.sessions',
    # 'django.contrib.messages',
    'django.contrib.staticfiles',
    "app01.apps.App01Config",
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    # 'django.contrib.auth.middleware.AuthenticationMiddleware',
    # 'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]


# session
SESSION_ENGINE = 'django.contrib.sessions.backends.db'

SESSION_COOKIE_NAME = "sid"  # Session的cookie保存在浏览器上时的key，即：sessionid＝随机字符串
SESSION_COOKIE_PATH = "/"  # Session的cookie保存的路径
SESSION_COOKIE_DOMAIN = None  # Session的cookie保存的域名
SESSION_COOKIE_SECURE = False  # 是否Https传输cookie
SESSION_COOKIE_HTTPONLY = True  # 是否Session的cookie只支持http传输
SESSION_COOKIE_AGE = 1209600  # Session的cookie失效日期（2周）

SESSION_EXPIRE_AT_BROWSER_CLOSE = False  # 是否关闭浏览器使得Session过期
SESSION_SAVE_EVERY_REQUEST = True  # 是否每次请求都保存Session，默认修改之后才保存
```

![](/post/Python/Django/1732337591554-c318f336-ede4-418c-a048-ad2c07c652c4.png)

+ 缓存

```python
INSTALLED_APPS = [
    # 'django.contrib.admin',
    # 'django.contrib.auth',
    # 'django.contrib.contenttypes',
    # 'django.contrib.sessions',
    # 'django.contrib.messages',
    'django.contrib.staticfiles',
    "app01.apps.App01Config",
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    # 'django.contrib.auth.middleware.AuthenticationMiddleware',
    # 'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]


# session
SESSION_ENGINE = 'django.contrib.sessions.backends.cache'
SESSION_CACHE_ALIAS = 'default' 

SESSION_COOKIE_NAME = "sid"  # Session的cookie保存在浏览器上时的key，即：sessionid＝随机字符串
SESSION_COOKIE_PATH = "/"  # Session的cookie保存的路径
SESSION_COOKIE_DOMAIN = None  # Session的cookie保存的域名
SESSION_COOKIE_SECURE = False  # 是否Https传输cookie
SESSION_COOKIE_HTTPONLY = True  # 是否Session的cookie只支持http传输
SESSION_COOKIE_AGE = 1209600  # Session的cookie失效日期（2周）

SESSION_EXPIRE_AT_BROWSER_CLOSE = False  # 是否关闭浏览器使得Session过期
SESSION_SAVE_EVERY_REQUEST = True  # 是否每次请求都保存Session，默认修改之后才保存
```

### 缓存
+ 服务器 + redis安装启动
+ django
    - 安装连接redis包

```python
pip install django-redis
```

    - settings.py

```python
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
            "CONNECTION_POOL_KWARGS": {"max_connections": 100}
            # "PASSWORD": "密码",
        }
    }
}
```

    - 手动操作redis

```python
from django_redis import get_redis_connection

conn = get_redis_connection("default")
conn.set("xx","123123")
conn.get("xx")
```