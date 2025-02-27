---
title: "DRF"
slug: "drf"
description: "django用于快速编写API的app"
date: "2025-02-27T14:18:29+08:00"
lastmod: "2025-02-27T14:18:29+08:00"
image: 
categories: ["python"]
tags: ["DRF","python"]
---
<!--more-->

## 1. 前言

### 1.1 前后端分离

![](post\python\DRF\1733982054512-99900304-27f3-48bb-aa08-adf52759a427.png)

![](post\python\DRF\1733982078114-e0b608b9-4f16-4e15-a508-747a37443a76.png)



### 1.2 django的FBV和CBV

+ `FBV：function base views`，即编写函数来处理业务请求（函数）

```python
from django.contrib import admin
from django.urls import path
from app01 import views
urlpatterns = [
    path('users/', views.users),
]
```

```python
from django.http import JsonResponse

def users(request,*args, **kwargs):
    if request.method == "GET":
        return JsonResponse({"code":1000,"data":"xxx"})
    elif request.method == 'POST':
        return JsonResponse({"code":1000,"data":"xxx"})
    ...
```

+ `CBV，class base views`，即编写类来处理业务请求（类加反射）

```python
from django.contrib import admin
from django.urls import path
from app01 import views
urlpatterns = [
    path('users/', views.UserView.as_view()),
]
```

```python
from django.views import View

class UserView(View):
    def get(self, request, *args, **kwargs):
        return JsonResponse({"code": 1000, "data": "xxx"})

    def post(self, request, *args, **kwargs):
        return JsonResponse({"code": 1000, "data": "xxx"})
```

其实，`CBV`和`FBV`的底层实现本质上相同的。

### 1.3 drf

```python
# django中的CBV
class View()
# drf框架中的CBV
class APIViews(View)
# 自定义的类
class UserInfo(APIViews)
```

## 2. 创建项目

### 2.1 django配置

+ 创建纯净python项目并配置虚拟环境

![](post\python\DRF\1733983824945-78126a3b-6ded-4f6c-aeb6-8cc02cb2da76.png)

+ 安装django库并完成基础的项目配置

```python
# 安装django库
pip install django==3.2
# 创建项目目录
django-admin startproject day13 .
# 创建一个app
python manage.py startapp api
```

+ 纯净版django项目（对settings.py文件操作）

```python
INSTALLED_APPS = [
    # 'django.contrib.admin',
    # 'django.contrib.auth',
    # 'django.contrib.contenttypes',
    # 'django.contrib.sessions',
    # 'django.contrib.messages',
    'django.contrib.staticfiles',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    # 'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    # 'django.middleware.csrf.CsrfViewMiddleware',
    # 'django.contrib.auth.middleware.AuthenticationMiddleware',
    # 'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
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

+ 配置项目启动按钮

![](post\python\DRF\1733984751636-3187b259-2c2d-4914-965a-e45893bd79ed.png)

![](post\python\DRF\1733984879376-354e1923-5d2a-46e1-bbc4-1f33db967ac0.png)

![](post\python\DRF\1733984996746-4c8d1633-72c5-4cd2-8848-5adf2d7cdf34.png)

![](post\python\DRF\1733985045978-a462498f-b2ba-4da9-803c-29176ef07abc.png)

+ 测试简单实例

```python
# api/views.py

from django.shortcuts import render, HttpResponse


def home(request):
    return HttpResponse("成功")
```

```python
# day13/urls.py

# from django.contrib import admin
from django.urls import path
from api import views

urlpatterns = [
    # path('admin/', admin.site.urls),
    path('home/', views.home),
]
```

### 2.2 配置drf

+ 安装drf框架

```python
pip install djangorestframework==3.13.1
```

+ settings.py配置

```python
INSTALLED_APPS = [
    # 'django.contrib.admin',
    # 'django.contrib.auth',
    # 'django.contrib.contenttypes',
    # 'django.contrib.sessions',
    # 'django.contrib.messages',
    'django.contrib.staticfiles',
    'api.apps.ApiConfig',
    'rest_framework',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    # 'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    # 'django.contrib.auth.middleware.AuthenticationMiddleware',
    # 'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]


TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
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

################## drf匿名用户  ##################
# 在request中源码可以找到
REST_FRAMEWORK = {
    "UNAUTHENTICATED_USER": None
}
```

+ 简单实例

```python
# from django.contrib import admin
from django.urls import path
from api import views

urlpatterns = [
    # path('admin/', admin.site.urls),
    path('home/', views.home),
    path('user/', views.UserView.as_view()),
]
```

```python
from django.shortcuts import render, HttpResponse
from rest_framework.views import APIView
from rest_framework.response import Response


def home(request):
    return HttpResponse("成功")


class UserView(APIView):
    def get(self, request):
        return Response("返回成功")

```



## 3. request

### 3.1 oop知识

- `getattr`：

- `__getattr__`：
  - 只有在属性查找失败时才会被调用（即在对象的 `__dict__` 和类的 `__dict__` 中都找不到属性时）。
  - 它是一个“后备”方法，通常用于动态地提供属性或方法。
- `__getattribute__`：
  - 是一个更底层的方法，会在每次属性访问时被调用，无论属性是否存在。
  - 它的优先级高于 `__getattr__`，并且可以完全控制属性的访问行为。



+ 直接访问或利用反射

```python
class Foo(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def show(self):
        return 123


obj = Foo("小三", 19)
# obj.name
# obj.age
# obj.show()

# v1=obj.show
v1 = getattr(obj, 'show')  # <bound method Foo.show of <__main__.Foo object at 0x000001D9870DBFD0>>
print(v1())
```

+ `__getattr__`
  - 当对象.一个不存在的成员时触发

```python
class Foo(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def show(self):
        return 123

    def __getattr__(self, item):
        print("----->", item)
        return 999


obj = Foo("小三", 19)

# 不触发 __getattr
# obj.name
# obj.age
# obj.show()

# 触发 __getattr__ (不存在的成员)
# print(obj.xxx)
v2 = getattr(obj, "xxxx")
print(v2)
# -----> xxxx
# 999
```

+ __getattribute__
  - 只要执行 对象.xxx都会执行__getattribute__
  - 父类object中的__getattribute__的处理机制
    * 对象中有值，返回
    * 对象中无值，报错

```python
class Foo(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def show(self):
        return 123

    def __getattribute__(self, item):
        print("----->", item)
        return 999


obj = Foo("小三", 19)

print(obj.name)
print(obj.age)
print(obj.xxx)

# -----> name
# 999
# -----> age
# 999
# -----> xxx
# 999
```

+ 类分析

```python
class HttpRequest(object):
    def __init__(self):
        pass

    def v1(self):
        print("v1")

    def v2(self):
        print("v2")


class Request(object):
    def __init__(self, req, xx):
        self._request = req
        self.xx = xx


req = HttpRequest()
req.v1()
req.v2()

request = Request(req, 111)
request._request.v1()
request._request.v2()
```

```python
class HttpRequest(object):
    def __init__(self):
        pass

    def v1(self):
        print("v1")

    def v2(self):
        print("v2")


class Request(object):
    def __init__(self, req, xx):
        self._request = req
        self.xx = xx

    # 对象中无的成员，会触发
    def __getattr__(self, attr):
        # attr="v1"
        try:
            return getattr(self._request, attr)
        except AttributeError:
            return self.__getattribute__(attr)


req = HttpRequest()

request = Request(req, 111)
# request._request.v1()
request.v1()  # request.v1=request._request.v1
```

+ request的简单实现原理（属性代理）

```python
class HttpRequest(object):
    def __init__(self):
        pass

    def method(self):
        print("v1")

    def path_info(self):
        print("v2")


class DrfRequest(object):
    def __init__(self, req, xx):
        self._request = req
        self.xx = xx

    def __getattr__(self, attr):
        try:
            return getattr(self._request, attr)
        except AttributeError:
            return self.__getattribute__(attr)


request = HttpRequest()
request = DrfRequest(request, 123123)

# 普通写法
request._request.method
# 利用__getattr__和__getattribute__
request.method
request.path_info
# 报错
request.uuuu

```

### 3.2 参数

普通匹配：`<数据类型:参数名>`，视图直接用`参数`接收，或者`self.kwargs`中获取

![](post\python\DRF\1734150636720-5cac42a1-7030-46db-950d-cda1559234b2.png)

不指定参数名正则匹配（导入re_path）：`(正则表达式)`，视图直接用`参数`接收，或者`self.args`中获取

![](post\python\DRF\1734150642488-23de7af7-4501-423a-a4ce-35d03662d913.png)

指定参数名正则匹配：`(?P<参数名>正则表达式)`，视图直接用`参数`接收，或者`self.kwargs`中获取

命名捕获组（Named Capture Group）：`(?P<参数名>正则表达式)`

![](post\python\DRF\1734150651203-61c81235-6ac7-49d7-ae8d-32234589fdfe.png)

- `测试代码`

```python
from django.contrib import admin
from django.urls import path, re_path
from api import views

urlpatterns = [
    # path('admin/', admin.site.urls),
    # path('users/<str:version>/<int:pid>/', views.UserView.as_view()),
    # re_path('users/(\w+)/(\d+)/', views.UserView.as_view()),
    re_path('users/(?P<version>\w+)/(?P<pid>\d+)/', views.UserView.as_view()),
]
```

```python
from rest_framework.views import APIView
from rest_framework.response import Response


class UserView(APIView):
    def get(self, request, version, pid):
        print(version, pid)
        print(self.kwargs)
        print(self.args)
        return Response("...")

```

### 3.3 源码分析

![](post\python\DRF\1734153342930-18d2c2fe-47b0-4bdf-a768-d6145989fd3b.png)

- `流程梳理-文字版`

```text
注意：下列调用方法的找寻路线遵循先UserView类，后APIView类，最后是View类，就不再详细赘述找寻过程

首先入口是路由中调用UserView类的as_View()方法，找到APIView类的as_view()方法，又调用View类的as_view()方法
View类的as_view()方法调用APIView类的dispatch方法

dispatch()方法调用initialize_request()方法，将django的request传给drf的Request类，Request 的构造函数接收上述参数，并将它们存储在实例中，并且定义了__getattr__方法，用于在drf的request中无缝访问原生django的request

此时initialize_request()方法调用完毕，回到dispatch()方法

dispatch()通过getattr反射获取请求方法，并调用，将结果一步步返回
```

- `源代码分析-精简版`

```python
class APIView(View):
    def as_view(cls, **initkwargs):
        view = super().as_view(**initkwargs)
        return csrf_exempt(view)

    def initialize_request(self, request, *args, **kwargs):
        parser_context = self.get_parser_context(request)

        return Request(
            request,
            parsers=self.get_parsers(),
            authenticators=self.get_authenticators(),
            negotiator=self.get_content_negotiator(),
            parser_context=parser_context
        )

    def dispatch(self, request, *args, **kwargs):
        self.args = args
        self.kwargs = kwargs
        # request还是django中的request
        request = self.initialize_request(request, *args, **kwargs)
        # 此时的request时drf的request，request._request就是原生django的request
        # request._request.method
        # request.method  # __getattr__
        self.request = request
        self.headers = self.default_response_headers

        if request.method.lower() in self.http_method_names:
            # 获取get/post/put等方法
            handler = getattr(self, request.method.lower(),self.http_method_not_allowed)
        # 调用get/post等方法(UserView中)
        response = handler(request, *args, **kwargs)

        self.response = self.finalize_response(request, response, *args, **kwargs)
        return self.response


class View:
    def as_view(cls, **initkwargs):
        def view(request, *args, **kwargs):
            return self.dispatch(request, *args, **kwargs)

        return view


class Request:
    def __init__(self, request, parsers=None, authenticators=None,
                 negotiator=None, parser_context=None):
        self._request = request

    def __getattr__(self, attr):

        try:
            return getattr(self._request, attr)
        except AttributeError:
            return self.__getattribute__(attr)
        
        
class UserView(APIView):
    def get(self, request, version, pid):
        print(version, pid)
        print(self.kwargs)
        print(self.args)
        return Response("...")

```



### 3.4 request对象

- Django 原生 `HttpRequest`
  - `django.core.handlers.wsgi.WSGIRequest`
  - 包含了请求的所有数据
    - `request.method`：请求方法（GET、POST 等）
    - `request.GET` 和 `request.POST`：查询参数和表单数据
    - `request.body`：请求体的原始数据

- DRF 的 `Request` 对象
  - `rest_framework.request.Request`
  - 提供了更灵活的方式来处理请求数据
    - `self._request`：存储 Django 的原生 `HttpRequest` 对象
    - `request.query_params`：用于获取 URL 查询参数（`GET` 请求的参数）
    - `request.data`：用于获取请求体中的数据，支持多种格式（如 JSON、表单数据等）
    - `request.method`：获取请求方法（如 GET、POST、PUT 等），与 Django 的 `request.method` 功能相同
    - `request.user` 和 `request.auth`：当前请求的用户对象，经过认证后的用户信息和认证相关的额外信息，例如 Token
    - `request.parsers`：当前请求使用的解析器列表，用于解析请求体数据
    - `request.authenticators`：当前请求使用的认证器列表
    - `request.is_ajax()`：判断请求是否为 AJAX 请求
    - `request.accepted_renderer`：获取当前请求的渲染器，用于决定响应数据的格式
    - `request.accepted_media_type`：获取客户端请求的 `Accept` 头部信息，用于内容协商
    - `request.stream`：获取请求体的原始流数据

drf中的request其实是对请求的再次封装，其目的就是在原来的request对象基础中再进行封装一些drf中需要用到的值

![](post\python\DRF\1734153473029-fd2d91cd-6312-4f75-9888-afc49b5dfc8f.png)

- `测试代码`

```python
from django.contrib import admin
from django.urls import path, re_path
from api import views

urlpatterns = [
    # path('admin/', admin.site.urls),
    # path('users/<str:version>/<int:pid>/', views.UserView.as_view()),
    # re_path('users/(\w+)/(\d+)/', views.UserView.as_view()),
    re_path('users/(?P<version>\w+)/(?P<pid>\d+)/', views.UserView.as_view()),
]
```

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.request import Request


class UserView(APIView):
    def get(self, request, version, pid):
        # drf的request对象
        print(request.query_params)
        print(request.data)
        print(request.auth)
        print(request.user)

        # django的request对象
        print(request.GET)
        print(request.method)
        print(request.path_info)
        return Response("...")
```

## 5. 认证

在开发API过程中，有些功能需要登录才能访问，有些无需登录。drf中的认证组件主要就是用来实现此功能

### 5.1 快速使用

通过编写类的形式实现，一个类就是一个认证组件

+ 编写类 -> 认证组件
+ 应用组件

#### 案例1

> 项目要开发3个接口，其中1个无需登录接口、2个必须登录才能访问的接口。
>
> 局部配置 authentication_classes = []

无需登录的不配置认证类，默认是匿名用户，需要登陆的配置认证类，通过返回元组赋值`request.user` 和 `request.auth`，未通过抛出异常。若返回None则是匿名用户，不符合需求

+ 认证失败信息定制

![](post\python\DRF\1734157419487-92ad7693-d15f-46a1-af5a-f403b0e0358b.png)

+ 匿名用户

![](post\python\DRF\1734157650997-6ee13a51-5b0b-4f8b-bf96-c8e051217c05.png)

+ 认证组件中返回的两个值，分别赋值给：`request.user` 和 `request.auth`

![](post\python\DRF\1734157774032-eab61203-3c75-47c0-9b85-9273a5af1fbf.png)

- `urls.py`

```python
from django.urls import path
from api import views

urlpatterns = [
    path('login/', views.LoginView.as_view()),
    path('user/', views.UserView.as_view()),
    path('order/', views.OrderView.as_view()),

]
```

- `views.py`

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.request import Request
from rest_framework.authentication import BaseAuthentication
from rest_framework.exceptions import AuthenticationFailed


class MyAuthentication(BaseAuthentication):
    def authenticate(self, request):
        # /xxx/xxx/xxx?token=123123123
        # token = request._request.GET.get('token')
        token = request.query_params.get('token')

        if token:
            return "user", token
        # raise AuthenticationFailed("认证失败")
        raise AuthenticationFailed({"code": 20000, "error": "认证失败"})


class LoginView(APIView):
    authentication_classes = []

    def get(self, request):
        print(request.user, request.auth)
        return Response("LoginView")


class UserView(APIView):
    authentication_classes = [MyAuthentication, ]

    def get(self, request):
        print(request.user, request.auth)
        return Response("UserView")


class OrderView(APIView):
    authentication_classes = [MyAuthentication, ]

    def get(self, request):
        print(request.user, request.auth)
        return Response("OrderView")

```



#### 案例2

> 项目要开发100个接口，其中1个无需登录接口、99个必须登录才能访问的接口。
>
> 此时，就需要用到drf的全局配置（认证组件的类不能放在视图view.py中，会因为导入APIView导致循环引用，该案例放置在ext/auth.py中）
>
> drf会先加载全局配置，在加载局部配置，局部配置会覆盖全局配置

- `ext\auth.py`

```python
from rest_framework.authentication import BaseAuthentication
from rest_framework.exceptions import AuthenticationFailed


class MyAuthentication(BaseAuthentication):
    def authenticate(self, request):
        # /xxx/xxx/xxx?token=123123123
        # token = request._request.GET.get('token')
        token = request.query_params.get('token')

        if token:
            return "user", token
        # raise AuthenticationFailed("认证失败")
        raise AuthenticationFailed({"code": 20000, "error": "认证失败"})

```

- `urls.py`

```python
from django.urls import path
from api import views

urlpatterns = [
    path('login/', views.LoginView.as_view()),
    path('user/', views.UserView.as_view()),
    path('order/', views.OrderView.as_view()),

]
```

- `views.py`

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from ext.auth import MyAuthentication


class LoginView(APIView):
    authentication_classes = []

    def get(self, request):
        print(request.user, request.auth)
        return Response("LoginView")


class UserView(APIView):

    def get(self, request):
        print(request.user, request.auth)
        return Response("UserView")


class OrderView(APIView):

    def get(self, request):
        print(request.user, request.auth)
        return Response("OrderView")

```

#### 案例3

> 状态码一致性问题：
> 如果认证组件中未定义，则统一为 HTTP_403_FORBIDDEN
>
> 如果自定义了，还会加上一个响应头

定义`get_authenticate_header`方法

![image-20241215160350164](post\python\DRF\image-20241215160350164.png)

![image-20241215162322059](post\python\DRF\image-20241215162322059.png)

- 参考源码

```python
def handle_exception(self, exc):

        if isinstance(exc, (exceptions.NotAuthenticated,exceptions.AuthenticationFailed)):
            auth_header = self.get_authenticate_header(self.request)
            if auth_header:
                exc.auth_header = auth_header
            else:
                exc.status_code = status.HTTP_403_FORBIDDEN
        return response

    def get_authenticate_header(self, request):
        authenticators = self.get_authenticators()
        if authenticators:
            return authenticators[0].authenticate_header(request)
```

#### 案例4

> 项目要开发100个接口，其中1个无需登录接口、98个必须登录才能访问的接口、1个公共接口（未登录时显示公共/已登录时显示个人信息）。
>
> 原来的认证信息只能放在URL中传递，如果程序中支持放在很多地方，例如：URL中、请求头中等。
>
> 认证组件中，如果是使用了多个认证类，会按照顺序逐一执行其中的`authenticate`方法
>
> - 返回None或无返回值，表示继续执行后续的认证类
> - 返回   (user, auth) 元组，则不再继续并将值赋值给request.user和request.auth
> - 抛出异常 `AuthenticationFailed(...)`，认证失败，不再继续向后走。
>
> 多个认证组件每一个都返回None，都没有认证成功，视图函数也会被执行，只不过`self.user，self.auth`为None

- `ext\auth.py`

设置一个检查参数的认证类，一个检查请求头的认证类，通过一个即为认证成功，然后设置一个认证类直接抛出异常

对于无需登陆的，局部设置无认证类

对于需要登陆的，则将所有认证类加上，若类1与类2未通过，走到类3会直接抛出异常即认证不通过

对于公共接口，局部设置前两个认证类，若未通过则以匿名用户访问

```python
from rest_framework.authentication import BaseAuthentication
from rest_framework.exceptions import AuthenticationFailed


class ParamAuthentication(BaseAuthentication):
    def authenticate(self, request):
        token = request.query_params.get('token')

        if not token:
            return
        return "user", token

    def authenticate_header(self, request):
        return 'Token'


class HeaderAuthentication(BaseAuthentication):
    def authenticate(self, request):
        token = request.META.get('HTTP_AUTHORIZATION')

        if not token:
            return
        return "user", token

    def authenticate_header(self, request):
        return 'Token'


class NotAuthentication(BaseAuthentication):
    def authenticate(self, request):
        raise AuthenticationFailed({"code": 20000, "error": "认证失败"})

    def authenticate_header(self, request):
        return 'Token'

```

- `views.py`

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from ext.auth import ParamAuthentication, HeaderAuthentication


# 无需登录
class UserView(APIView):
    authentication_classes = []

    def get(self, request):
        return Response("用户信息")


# 公共接口
class OrderView(APIView):
    authentication_classes = [ParamAuthentication, HeaderAuthentication, ]

    def get(self, request):
        if request.user:
            message = f"{request.user}的订单信息"

        else:
            message = "公共订单信息"
        return Response(message)


# 必须登录
class InfoView(APIView):
    def get(self, request):
        message = f"{request.user}的用户信息"
        return Response(message)

```

- `settings.py`

```python
REST_FRAMEWORK = {
    "UNAUTHENTICATED_USER": None,  # 未登录时，request.user=None
    "UNAUTHENTICATED_TOKEN": None,  # 未登录时，request.auth=None
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "ext.auth.ParamAuthentication",
        "ext.auth.HeaderAuthentication",
        "ext.auth.NotAuthentication",
    ]
}
```



### 5.2 面向对象-继承

局部配置与全局配置的优先关系

```python
class APIView(View):
    authentication_classes = 读取配置文件中的列表

    def dispatch(self):
        self.authentication_classes


class UserView(APIView):
    authentication_classes = [11,22,33,44]


obj = UserView()
obj.dispatch()  # authentication_classes = [11,22,33,44]
```



### 5.3 组件源码

```
在request的初始化阶段加载认证组件，即调用initialize_request方法时，通过get_authenticators方法获取认证类实例对象列表authenticators
	get_authenticators方法循环authentication_classes，实例化生成认证类实例对象存储在列表中
	authentication_classes读取的局部配置和全局配置
加载完成后，认证过程，即调用initial方法时，调用perform_authentication方法，获取request.user的值
	user为空值即未认证，触发_authenticate方法，循环认证类实例对象列表authenticators，调用自己的认证方法，返回元组就赋值user和auth，退出循环；返回None继续循环；抛出异常就捕获异常，在dispatch中捕获；如果循环完没返回元组也没异常，执行_not_authenticated方法处理匿名用户
```



![无标题-2024-11-16-0939](post\python\DRF\无标题-2024-11-16-0939.png)



```python
class Request:
    def __init__(self, request, authenticators=None):
        self._request = request
        self.authenticators = authenticators or ()

    @property
    def user(self):
        if not hasattr(self, '_user'):
            with wrap_attributeerrors():
                self._authenticate()
        return self._user

    @user.setter
    def user(self, value):
        self._user = value
        self._request.user = value

    def _authenticate(self):
        # authenticator是认证类实例化后的对象，循环逐一认证
        # 如果认证过程抛出异常，认证不通过
        # 若认证过程没有抛出异常，也没有返回self.user, self.auth，则匿名执行视图函数
        for authenticator in self.authenticators:
            try:
                user_auth_tuple = authenticator.authenticate(self)  # 调用认证类的authenticate方法，返回元组
            except exceptions.APIException:
                self._not_authenticated()
                raise

            if user_auth_tuple is not None:
                self._authenticator = authenticator
                self.user, self.auth = user_auth_tuple
                return

        self._not_authenticated()

    def _not_authenticated(self):
        self._authenticator = None
        if api_settings.UNAUTHENTICATED_USER:
            self.user = api_settings.UNAUTHENTICATED_USER()
        else:
            self.user = None

        if api_settings.UNAUTHENTICATED_TOKEN:
            self.auth = api_settings.UNAUTHENTICATED_TOKEN()
        else:
            self.auth = None


class APIView(View):
    authentication_classes = api_settings.DEFAULT_AUTHENTICATION_CLASSES

    def as_view(cls, **initkwargs):
        view = super().as_view(**initkwargs)
        return csrf_exempt(view)

    def dispatch(self, request, *args, **kwargs):
        # 第一步：请求的封装(django的 request + drf的 authenticators认证组件)  --> 加载认证组件过程
        # request.authenticators  [对象1，对象2，...]
        request = self.initialize_request(request, *args, **kwargs)
        self.request = request
        # 第二步：认证过程
        try:
            self.initial(request, *args, **kwargs)
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
            # 第三步：执行视图函数
            response = handler(request, *args, **kwargs)
        except Exception as exc:  # 认证时抛出错误在这里被捕获
            response = self.handle_exception(exc)
        self.response = self.finalize_response(request, response, *args, **kwargs)
        return self.response

    def initialize_request(self, request, *args, **kwargs):
        return Request(
            request,
            authenticators=self.get_authenticators(),  # [对象1，对象2，...]
        )

    def initial(self, request, *args, **kwargs):
        self.perform_authentication(request)
        self.check_permissions(request)
        self.check_throttles(request)

    def perform_authentication(self, request):
        request.user

    def get_authenticators(self):
        # 返回一个列表每一个元素都是认证类对象 [对象1，对象2，...]
        return [auth() for auth in self.authentication_classes]  # 优先读取局部配置，后全局配置

    def handle_exception(self, exc):

        if isinstance(exc, (exceptions.NotAuthenticated,exceptions.AuthenticationFailed)):
            auth_header = self.get_authenticate_header(self.request)
            if auth_header:
                exc.auth_header = auth_header
            else:
                exc.status_code = status.HTTP_403_FORBIDDEN
        return response

    def get_authenticate_header(self, request):
        authenticators = self.get_authenticators()
        if authenticators:
            return authenticators[0].authenticate_header(request)

class View:
    def as_view(cls, **initkwargs):
        def view(request, *args, **kwargs):
            return self.dispatch(request, *args, **kwargs)

        return view


class UserView(APIView):
    def get(self, request):
        print(request.user, request.auth)
        return Response("UserView")

```

### 5.4 拓展-子类约束

```python
class Foo(object):
    # 对子类进行约束，约束子类中必须要定义该方法，不然就报错
    def f1(self):
        raise NotImplementedError("...")


class Bar(Foo):
    def f1(self):
        print("123")
```

- `BaseAuthentication类定义`

```python
class BaseAuthentication:
    """
    All authentication classes should extend BaseAuthentication.
    """

    def authenticate(self, request):
        """
        Authenticate the request and return a two-tuple of (user, token).
        """
        raise NotImplementedError(".authenticate() must be overridden.")
```



### 5.5 案例-用户登录和认证

#### 5.5.1 创建数据表

> 不配置其他数据库，采用默认的`sqlite`做简单测试

- `注册app --> settings.py`

```python
INSTALLED_APPS = [
    # 'django.contrib.admin',
    # 'django.contrib.auth',
    # 'django.contrib.contenttypes',
    # 'django.contrib.sessions',
    # 'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'api.apps.ApiConfig',
]
```



- `api/models.py`

```python
from django.db import models


# Create your models here.
class UserInfo(models.Model):
    """用户表"""
    username = models.CharField(verbose_name="用户名", max_length=32)
    password = models.CharField(verbose_name="用户名", max_length=64)
    # 临时形式、后续可以使用jwt等
    token = models.CharField(verbose_name="TOKEN", max_length=64, null=True, blank=True)
```

- 创建数据库

![image-20241215214124221](post\python\DRF\image-20241215214124221.png)

- 手动添加数据做测试

![image-20241215214406794](post\python\DRF\image-20241215214406794.png)

#### 5.5.2 路由系统

```python
from django.urls import path
from api import views

urlpatterns = [
    path('login/', views.LoginView.as_view()),
    path('user/', views.UserView.as_view()),
    path('order/', views.OrderView.as_view()),

]
```



#### 5.5.3 认证组件

> 实际开发过程中应定制响应状态码，该案例使用简单的True和False标识状态

- `settings.py全局配置`

```python
REST_FRAMEWORK = {
    "UNAUTHENTICATED_USER": None,  # 未登录时，request.user=None
    "UNAUTHENTICATED_TOKEN": None,  # 未登录时，request.auth=None
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "ext.auth.QueryParamsAuthentication",
        "ext.auth.HeaderAuthentication",
        "ext.auth.NoAuthentication",
    ]
}
```

- `ext/auth.py`

```python
from rest_framework.authentication import BaseAuthentication
from rest_framework.exceptions import AuthenticationFailed
from api import models


class QueryParamsAuthentication(BaseAuthentication):
    def authenticate(self, request):
        # 获取请求参数值
        token = request.query_params.get('token')
        if not token:
            return
        user_object = models.UserInfo.objects.filter(token=token).first()
        if user_object:
            return user_object, token  # request.user = 用户对象，request.auth = token

        return

    def authenticate_header(self, request):
        return 'Token'


class HeaderAuthentication(BaseAuthentication):
    def authenticate(self, request):
        # 获取请求头
        token = request.META.get('HTTP_AUTHORIZATION')
        if not token:
            return
        user_object = models.UserInfo.objects.filter(token=token).first()
        if user_object:
            return user_object, token  # request.user = 用户对象，request.auth = token

        return

    def authenticate_header(self, request):
        return 'Token'


class NoAuthentication(BaseAuthentication):
    def authenticate(self, request):
        raise AuthenticationFailed({"status": False, "msg": "认证失败"})

    def authenticate_header(self, request):
        return 'Token'

```



#### 5.5.4 视图函数

- postman发送post请求测试接口示例

![image-20241215215621539](post\python\DRF\image-20241215215621539.png)

- 登录生成token接口

```python
import uuid
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.request import Request
from api import models


class LoginView(APIView):
    authentication_classes = []  # 局部配置，该函数无需登录

    def post(self, request):
        # 1.接收用户post提交的用户名密码
        # request.data直接是一个字典类型
        user = request.data.get('username')
        pwd = request.data.get('password')
        # 2.数据库校验
        user_object = models.UserInfo.objects.filter(username=user, password=pwd).first()
        if not user_object:
            return Response({"status": False, "msg": "用户名或密码错误"})

        # 3.登陆成功
        token = str(uuid.uuid4())
        user_object.token = token
        user_object.save()
        return Response({"status": True, "data": token})
```

![image-20241215221444282](post\python\DRF\image-20241215221444282.png)

- 需要认证的视图函数

```python
class UserView(APIView):
    def get(self, request):
        return Response("UserView")
```

#### 5.5.5 访问效果

> 在请求头中携带和在请求参数中携带均可，有对应的认证组件校验

![image-20241215223514300](post\python\DRF\image-20241215223514300.png)

### 5.6 总结

编写组件

```python
# ext/auth.py
from rest_framework.authentication import BaseAuthentication
from rest_framework.exceptions import AuthenticationFailed


class MyAuthentication(BaseAuthentication):
    def authenticate(self, request):
        # 去做用户认证
        # 1.读取请求传递的token
        token = request.query_params.get('token')
        # 2.校验合法性
        # 3.返回值
        # 3.1 返回元组 (111,222) 认证成功 request.user request.auth
        # 3.2 抛出异常           认证失败 -> 返回错误信息
        # 3.1 返回None          多个认证类 [类1，类2，类3，类4，，，] -> 匿名用户
        if not token:
            return
        return "user", token

    def authenticate_header(self, request):
        return 'Token'
    
class NotAuthentication(BaseAuthentication):
    def authenticate(self, request):
        raise AuthenticationFailed({"code": 20000, "error": "认证失败"})

    def authenticate_header(self, request):
        return 'Token'
```

全局配置匿名用户和认证类

```python
# settings.py
REST_FRAMEWORK = {
    "UNAUTHENTICATED_USER": None,  # 未登录时，request.user=None
    "UNAUTHENTICATED_TOKEN": None,  # 未登录时，request.auth=None
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "ext.auth.ParamAuthentication",
        "ext.auth.HeaderAuthentication",
        "ext.auth.NotAuthentication",
    ]
}
```

局部配置认证类

```python
# views.py
class OrderView(APIView):
    authentication_classes = [... ]

    def get(self, request):
```



解决多认证参数传递（请求参数和请求头等等）和 必须认证，公共接口（可认证可匿名），无需认证三种类型接口 -->  见案例4



## 6. 权限

> - 认证组件 = [认证类，认证类，认证类，]		->	执行每一个认证类的authenticate方法
>
>   - 认证成功或失败，不会再执行后续的认证类
>
>   - 返回None，执行后续认证类
>
> - 权限组件 = [权限类，权限类，权限类，]        ->    执行所有权限类的has_permission方法，返回True通过，返回False不通过
>   - 执行所有的权限类
>
> 默认情况下，要保证所有的权限类中的has_permission方法都返回True，即`与`的关系
>
> 掌握源码流程后，可以扩展+自定义，实现其他逻辑



### 6.1 快速使用

- 编写类

```python
class BasePermission(metaclass=BasePermissionMetaclass):
    """
    A base class from which all permission classes should inherit.
    """

    def has_permission(self, request, view):
        """
        Return `True` if permission is granted, `False` otherwise.
        """
        return True
```

阅读源码可知，自定义的权限类的`has_permission`应返回`True或False`来表示是否有权限

```python
# ext/per.py
from rest_framework.permissions import BasePermission
import random


class MyPermission(BasePermission):
    def has_permission(self, request, view):
        # 获取请求中的数据，然后进行校验...
        v1 = random.randint(1, 3)
        if v1 == 2:
            return True
        return False
```

- 应用类

> 全局应用	在`settings.py`中配置
>
> 局部应用	在视图函数中定义`permission_classes = []`参数
>
> 根据业务需求灵活配置

```python
REST_FRAMEWORK = {
    "UNAUTHENTICATED_USER": None,  # 未登录时，request.user=None
    "UNAUTHENTICATED_TOKEN": None,  # 未登录时，request.auth=None
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "ext.auth.ParamAuthentication",
        "ext.auth.HeaderAuthentication",
        "ext.auth.NotAuthentication",
    ],
    "DEFAULT_PERMISSION_CLASSES":[
        "ext.per.Mypermission"
    ]
}
```

![image-20241217125147471](post\python\DRF\image-20241217125147471.png)

### 6.2 错误信息和多个权限类

- 在权限类中自定义错误信息`message`

```python
class MyPermission(BasePermission):
    message = {"status": False, "msg": "无权访问"}

    def has_permission(self, request, view):
        # 获取请求中的数据，然后进行校验...
        v1 = random.randint(1, 3)
        if v1 == 2:
            return True
        return False
```

左图为默认错误信息，右图为自定义错误信息

![image-20241217131658811](post\python\DRF\image-20241217131658811.png)

- 多个权限类

`api/per.py`

```python
from rest_framework.permissions import BasePermission


class MyPermission1(BasePermission):
    message = {"status": False, "msg": "无权访问"}

    def has_permission(self, request, view):
        print("MyPermission1")
        return False


class MyPermission2(BasePermission):
    message = {"status": False, "msg": "无权访问"}

    def has_permission(self, request, view):
        print("MyPermission2")
        return False


class MyPermission3(BasePermission):
    message = {"status": False, "msg": "无权访问"}

    def has_permission(self, request, view):
        print("MyPermission3")
        return False

```

`api/views.py`

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from ext.per import MyPermission1, MyPermission2, MyPermission3


class OrderView(APIView):
    authentication_classes = []
    permission_classes = [MyPermission1, MyPermission2, MyPermission3]

    def get(self, request):
        print(request.user, request.auth)
        return Response({"status": True, "data": [11, 22, 33, 44]})
```

终端只打印了`MyPermission1`

![image-20241217140459678](post\python\DRF\image-20241217140459678.png)

> 说明程序会按顺序校验每个权限类，一旦有一个返回False即校验不通过，则不再校验后续权限类，只有通过每一个权限类的校验才算有权限访问



### 6.3 源码流程

> as_view()等方法的调用在认证组件中已详细阐明，此处直接从dispatch()方法开始
>
> 初始化的initialize_request无涉及，直接来到initial方法，调用check_permissions方法
>
> check_permissions方法循环权限类实例对象列表permission_classes，调用has_permission方法，一旦返回False，就调用permission_denied方法，并传入错误信息message和code

```python
class APIView(View):
    permission_classes = api_settings.DEFAULT_PERMISSION_CLASSES  # 权限组件的全局配置

    def permission_denied(self, request, message=None, code=None):
        if request.authenticators and not request.successful_authenticator:
            raise exceptions.NotAuthenticated()
        raise exceptions.PermissionDenied(detail=message, code=code)

    def get_permissions(self):
        # 获取权限类对象的列表
        return [permission() for permission in self.permission_classes]

    def check_permissions(self, request):
        # 循环每一个权限类对象，判断权限校验是否通过
        for permission in self.get_permissions():
            # 如果未通过，则抛出异常在dispatch中捕获
            if not permission.has_permission(request, self):
                self.permission_denied(
                    request,
                    message=getattr(permission, 'message', None),
                    code=getattr(permission, 'code', None)
                )
    
    def initial(self, request, *args, **kwargs):
        self.perform_authentication(request)  # 认证组件的过程，循环执行每个authenticate方法；失败则抛出异常不向下执行
        self.check_permissions(request)  # 权限的校验
        self.check_throttles(request)

    def dispatch(self, request, *args, **kwargs):
        # 封装drf的request
        request = self.initialize_request(request, *args, **kwargs)
        self.request = request
        self.headers = self.default_response_headers  # deprecate?

        try:
            self.initial(request, *args, **kwargs)
            # 反射获取get/post等方法
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
            # 执行相应方法
            response = handler(request, *args, **kwargs)
        # 异常处理
        except Exception as exc:
            response = self.handle_exception(exc)
        self.response = self.finalize_response(request, response, *args, **kwargs)
        return self.response


class OrderView(APIView):
    permission_classes = [MyPermission1, MyPermission2, MyPermission3]  # 权限组件的局部配置

    def get(self, request):
        print(request.user, request.auth)
        return Response({"status": True, "data": [11, 22, 33, 44]})

```



### 6.4 组件的拓展

> 通过源码可以知道，在`check_permissions`中，只要有一个权限类校验不通过就是无权访问，那么如何才能实现拓展，使其满足只要有一个权限类通过就是有权限呢？显而易见需要重写`check_permissions`方法

```python
class OrderView(APIView):
    authentication_classes = []
    permission_classes = [MyPermission1, MyPermission2, MyPermission3]

    def get(self, request):
        print(request.user, request.auth)
        return Response({"status": True, "data": [11, 22, 33, 44]})

    def check_permissions(self, request):
        no_permission_objects_list = []
        for permission in self.get_permissions():
            if permission.has_permission(request, self):
                return
            else:
                no_permission_objects_list.append(permission)
        else:
            self.permission_denied(
                request,
                message=getattr(no_permission_objects_list[0], 'message', None),
                code=getattr(no_permission_objects_list[0], 'code', None)
            )
```

> 在`OrderView`视图中重写`check_permissions`方法，则在权限校验的生命周期中，自定义的`check_permissions`便覆盖了`APIView`中的默认`check_permissions`实现逻辑，达到自定义拓展的效果
>
> 自定义的`check_permissions`实现了一旦某一权限类校验通过，就不再进行后续校验。若所有类均未通过，则返回第一个未通过的类的错误信息
>
> 由此可见学习源码的重要性，不仅可以学习源码中优秀的代码编写方式，还可以在原有逻辑的基础上进行拓展

上述是修改一个视图函数的权限校验逻辑，如何应用到其他视图呢？

> 可以自定义一个`NbApiView`的类，继承`APIView`并且重写`check_permissions`，之后要实现或逻辑的权限认证的视图只需继承`NbApiView`即可

- `ext/view.py`

```python
from rest_framework.views import APIView


class NbApiView(APIView):
    def check_permissions(self, request):
        no_permission_objects_list = []
        for permission in self.get_permissions():
            if permission.has_permission(request, self):
                return
            else:
                no_permission_objects_list.append(permission)
        else:
            self.permission_denied(
                request,
                message=getattr(no_permission_objects_list[0], 'message', None),
                code=getattr(no_permission_objects_list[0], 'code', None)
            )
```

- api/views.py

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from ext.per import MyPermission1, MyPermission2, MyPermission3
from ext.view import NbApiView


class OrderView(NbApiView):
    authentication_classes = []
    permission_classes = [MyPermission1, MyPermission2, MyPermission3]

    def get(self, request):
        print(request.user, request.auth)
        return Response({"status": True, "data": [11, 22, 33, 44]})

```

### 6.5 案例-权限处理

#### 6.5.1 创建数据表

![image-20241217171036327](post\python\DRF\image-20241217171036327.png)

```python
class UserInfo(models.Model):
    """用户表"""
    role = models.IntegerField(verbose_name="角色", choices=((1, "总监"), (2, "经理"), (3, "员工")), default=3)
    username = models.CharField(verbose_name="用户名", max_length=32)
    password = models.CharField(verbose_name="用户名", max_length=64)
    # 临时形式、后续可以使用jwt等
    token = models.CharField(verbose_name="TOKEN", max_length=64, null=True, blank=True)
```

```python
makemigrations
migrate
```

手动添加数据

![image-20241217172759096](post\python\DRF\image-20241217172759096.png)



#### 6.5.2 路由系统

```python
from django.urls import path
from api import views

urlpatterns = [
    path('login/', views.LoginView.as_view()),
    path('user/', views.UserView.as_view()),
    path('order/', views.OrderView.as_view()),
    path('avatar/', views.AvatarView.as_view())
]
```



#### 6.5.3 认证组件

> 根据传入token返回用户对象

`ext/auth.py`

```python
from rest_framework.authentication import BaseAuthentication
from rest_framework.exceptions import AuthenticationFailed
from api import models


class QueryParamsAuthentication(BaseAuthentication):
    def authenticate(self, request):
        token = request.query_params.get('token')
        if not token:
            return
        user_object = models.UserInfo.objects.filter(token=token).first()
        if user_object:
            return user_object, token  # request.user = 用户对象，request.auth = token

        return

    def authenticate_header(self, request):
        return 'Token'


class HeaderAuthentication(BaseAuthentication):
    def authenticate(self, request):
        token = request.META.get('HTTP_AUTHORIZATION')
        if not token:
            return
        user_object = models.UserInfo.objects.filter(token=token).first()
        if user_object:
            return user_object, token  # request.user = 用户对象，request.auth = token

        return

    def authenticate_header(self, request):
        return 'Token'


class NoAuthentication(BaseAuthentication):
    def authenticate(self, request):
        raise AuthenticationFailed({"status": False, "msg": "认证失败"})

    def authenticate_header(self, request):
        return 'Token'

```

#### 6.5.4 权限组件

`ext/per.py`

```python
from rest_framework.permissions import BasePermission


class UserPermission(BasePermission):
    message = {"status": False, "msg": "无权访问1"}

    def has_permission(self, request, view):
        if request.user.role == 3:
            return True
        return False


class ManagerPermission(BasePermission):
    message = {"status": False, "msg": "无权访问1"}

    def has_permission(self, request, view):
        if request.user.role == 2:
            return True
        return False


class BossPermission(BasePermission):
    message = {"status": False, "msg": "无权访问1"}

    def has_permission(self, request, view):
        if request.user.role == 1:
            return True
        return False

```

`ext/view.py`

```python
from rest_framework.views import APIView


class NbApiView(APIView):
    def check_permissions(self, request):
        no_permission_objects_list = []
        for permission in self.get_permissions():
            if permission.has_permission(request, self):
                return
            else:
                no_permission_objects_list.append(permission)
        else:
            self.permission_denied(
                request,
                message=getattr(no_permission_objects_list[0], 'message', None),
                code=getattr(no_permission_objects_list[0], 'code', None)
            )

```

#### 6.5.5 视图函数

```python
import uuid
from rest_framework.views import APIView
from rest_framework.response import Response
from api import models
from ext.per import UserPermission, ManagerPermission, BossPermission
from ext.view import NbApiView


class LoginView(APIView):
    authentication_classes = []

    def post(self, request):
        # 1.接收用户post提交的用户名密码
        # print(request.query_params)  # 获取get请求提交的参数
        # request.data直接是一个字典类型
        user = request.data.get('username')
        pwd = request.data.get('password')
        # 2.数据库校验
        user_object = models.UserInfo.objects.filter(username=user, password=pwd).first()
        if not user_object:
            return Response({"status": False, "msg": "用户名或密码错误"})

        # 3.登陆成功
        token = str(uuid.uuid4())
        user_object.token = token
        user_object.save()
        return Response({"status": True, "data": token})


class UserView(NbApiView):
    # 经理、总检、普通用户都可以访问
    permission_classes = [BossPermission, ManagerPermission, UserPermission]

    def get(self, request):
        print(request.user, request.auth)
        return Response("UserView")


class OrderView(NbApiView):
    # 经理、总监可以访问
    permission_classes = [BossPermission, ManagerPermission, ]

    def get(self, request):
        print(request.user, request.auth)
        return Response({"status": True, "data": [11, 22, 33, 44]})


class AvatarView(NbApiView):
    # 总监、员工可以访问
    permission_classes = [BossPermission, UserPermission, ]

    def get(self, request):
        print(request.user, request.auth)
        return Response({"status": True, "data": [11, 22, 33, 44]})

```

#### 6.5.6 访问效果

> `token2`对应`经理`
>
> `token3`对应`员工`
>
> 员工可以访问`/avatar`,访问不了`/order`
>
> 经理可以访问`/order`,访问不了`/avatar`

![image-20241217173736707](post\python\DRF\image-20241217173736707.png)



### 6.6 思考

> 在开发过程中,发现drf中的request对象不好用,应该如何换成另一个request类对象?

```python
class AvatarView(NbApiView):
    # 总监、员工可以访问
    permission_classes = [BossPermission, UserPermission, ]

    def get(self, request):
        print(request.user, request.auth)
        return Response({"status": True, "data": [11, 22, 33, 44]})

    def initialize_request(self, request, *args, **kwargs):
        # super().initialize_request()
        parser_context = self.get_parser_context(request)
        return Request(
            request,
            parsers=self.get_parsers(),
            authenticators=self.get_authenticators(),
            negotiator=self.get_content_negotiator(),
            parser_context=parser_context
        )
# 再自己定义一个Request类即可完成接管,齐活!
```



> drf中的认证/权限组件与django中的中间件有什么关系?
>
> 认证/权限组件在中间件之后

![image-20241217182023566](post\python\DRF\image-20241217182023566.png)



### 6.7 总结

默认按权限类列表定义的顺序判断每一个权限，返回True继续判断，知道所有权限类均通过；返回False直接退出，无权限

通过修改check_permissions的逻辑可以实现，返回True直接退出，有权限；返回False继续判断，知道有True，若全为False，无权限

全局配置

```python
REST_FRAMEWORK = {
    "UNAUTHENTICATED_USER": None,  # 未登录时，request.user=None
    "UNAUTHENTICATED_TOKEN": None,  # 未登录时，request.auth=None
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "ext.auth.ParamAuthentication",
        "ext.auth.HeaderAuthentication",
        "ext.auth.NotAuthentication",
    ],
    "DEFAULT_PERMISSION_CLASSES":[
        "ext.per.Mypermission"
    ]
}
```

ext/per.py

```python
from rest_framework.permissions import BasePermission


class UserPermission(BasePermission):
    # 子定义错误信息
    message = {"status": False, "msg": "无权访问1"}
	# 权限判断，True或False
    def has_permission(self, request, view):
        if request.user.role == 3:
            return True
        return False

```

ext/view.py

```python
from rest_framework.views import APIView


class NbApiView(APIView):
    def check_permissions(self, request):
        no_permission_objects_list = []
        for permission in self.get_permissions():
            if permission.has_permission(request, self):
                return
            else:
                no_permission_objects_list.append(permission)
        else:
            self.permission_denied(
                request,
                message=getattr(no_permission_objects_list[0], 'message', None),
                code=getattr(no_permission_objects_list[0], 'code', None)
            )

```

视图函数

```python
from ext.per import UserPermission, ManagerPermission, BossPermission
from ext.view import NbApiView


# 继承自定义试图类，改变权限逻辑
class UserView(NbApiView):
	# 局部配置
    permission_classes = [BossPermission, ManagerPermission, UserPermission]

    def get(self, request):
```





## 7. 限流

### 7.1 基本逻辑

> 开发过程中，某个接口不想让用户访问过于频繁，需要限流机制
>
> 例如：平台发送验证码，除了云厂商提供的限制，可能还需要做其他限制，如IP限制、验证码等等
>
> 限制访问频率需要找到唯一标识:
>
> 		- 已登录用户，可以使用用户信息主键，ID，用户名
> 		- 未登录用户，可以采用IP作为唯一标识

如何限制？

维护一个`唯一标识：访问记录列表`

> `"121"：[ 20:35  20:32  20:22  20:12 ]`     限制10分钟3次
>
> 1. 获取当前时间  20:36
>
> 2. 当前时间-10分钟=计数开始时间  20：26
>
> 3. 删除小于20：26的记录
>
> 4. 计算长度
>
>    ​	4.1 超过，报错
>
>    ​	4.2 未超过，访问



### 7.2 快速使用

- 编写类

> 重写了获取唯一标识的get_cache_key方法，优先使用用户id，若没有再采用默认的获取请求ip做唯一标识
>
> 后续可以连接redis将标识做键，访问记录存储为对应的值进行联动

- `ext/throttle.py`

```python
from rest_framework.throttling import SimpleRateThrottle
from django.core.cache import cache as default_cache


class MyThrottle(SimpleRateThrottle):
    scope = "XXX"
    THROTTLE_RATES = {"XXX": "5/m"}  # 限制频率
    cache = default_cache  # redis配置

    # 获取唯一标识，后续可以连接redis做操作
    def get_cache_key(self, request, view):
        if request.user:
            ident = request.user.pk  # 用户ID
        else:
            ident = self.get_ident(request)  # 获取请求用户IP（去request中找请求头）
        # cache_format = 'throttle_%(scope)s_%(ident)s'
        return self.cache_format % {'scope': self.scope, 'ident': ident}

```

- redis配置

> 1.django-redis配置 	->	settings.py
> 2.安装django-redis	pip install django-redis
> 3.启动redis服务

```python
# settings.py
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
            "PASSWORD": "",
        }
    }
}
```

- 应用类

```python
from ext.throttle import MyThrottle

class LoginView(APIView):
    authentication_classes = []
    throttle_classes = [MyThrottle,]
    ...
```

![image-20241218093359538](post\python\DRF\image-20241218093359538.png)

### 7.3 源码流程

> 1. 对象加载
>
>    获取每个限流类对象，初始化（读取限流配置，获取时间间隔和访问次数）
>
> 2. allow_request判断是否限流

```
从dispatch方法的initial方法开始，调用了check_throttles方法
check_throttles方法中：
	self.get_throttles()，从局部配置或全局配置读取限流类列表throttle_classes，然后逐一实例化，返回一个限流类对象列表
	循环上述列表，调用每一个类的allow_request方法，如果返回False即被限流，将等待时间添加到列表throttle_durations中，并获取最大等待时间duration，然后抛出异常

对于每一个限流类，循环throttle_classes实例化时，重写了scope，THROTTLE_RATES，cache，get_cache_key等等属性方法，并调用__init__初始化
	调用get_rate方法，获取THROTTLE_RATES[self.scope]作为rate，如5/m
	调用parse_rate方法，将rate作为参数，返回次数与间隔给num_requests和duration
	
每一个类执行allow_request时，调用get_cache_key获取唯一标识key，从cache中根据key获取对应维护的记录列表给history，获取当前时间now，循环history，将时间小于now-duration即计数起点的记录pop掉，比较列表剩余长度与num_requests
	如果超过限制，调用throttle_failure
	正常访问，调用throttle_success
	
正常访问，throttle_success将本次记录加入history，并更新缓存，返回True
超过限制，调用throttle_failure返回False
```



```python
class BaseThrottle:
    def allow_request(self, request, view):
        raise NotImplementedError('.allow_request() must be overridden')

    def get_ident(self, request):
        pass

    def wait(self):
        return None


class SimpleRateThrottle(BaseThrottle):
    cache = default_cache
    timer = time.time
    cache_format = 'throttle_%(scope)s_%(ident)s'
    scope = None
    # 全局配置的scope与THROTTLE_RATES对应关系，可以在settings.py中配置DEFAULT_THROTTLE_RATES，在自定义限流类中只需指定scope即可
    THROTTLE_RATES = api_settings.DEFAULT_THROTTLE_RATES

    # 初始化访问限制的次数与时间
    def __init__(self):
        if not getattr(self, 'rate', None):
            self.rate = self.get_rate()  # "5/m"
        self.num_requests, self.duration = self.parse_rate(self.rate)

    def get_cache_key(self, request, view):
        raise NotImplementedError('.get_cache_key() must be overridden')

    def get_rate(self):
        if not getattr(self, 'scope', None):
            msg = ("You must set either `.scope` or `.rate` for '%s' throttle" %
                   self.__class__.__name__)
            raise ImproperlyConfigured(msg)

        try:
            # THROTTLE_RATES = {"XXX": "5/m"}  # 限制频率
            # scope = "XXX"
            return self.THROTTLE_RATES[self.scope]
        except KeyError:
            msg = "No default throttle rate set for '%s' scope" % self.scope
            raise ImproperlyConfigured(msg)

    # 将THROTTLE_RATES切割为次数和对应时间范围
    def parse_rate(self, rate):
        # "5/m"
        if rate is None:
            return (None, None)
        num, period = rate.split('/')
        num_requests = int(num)
        # period[0]取出s、m或h，因此自定义THROTTLE_RATES时也可以将h写成hour，因为它只读取索引为零的字符
        duration = {'s': 1, 'm': 60, 'h': 3600, 'd': 86400}[period[0]]
        return (num_requests, duration)  # 返回 5 60

    def allow_request(self, request, view):
        if self.rate is None:
            return True
        # 获取唯一标识
        self.key = self.get_cache_key(request, view)
        if self.key is None:
            return True
        # 获取历史访问记录[20:35  20:32  20:22  20:12]
        self.history = self.cache.get(self.key, [])
        # 获取当前时间
        self.now = self.timer()
        # 循环 将小于计数开始时间剔除
        while self.history and self.history[-1] <= self.now - self.duration:
            self.history.pop()
        # 计算长度
        if len(self.history) >= self.num_requests:
            return self.throttle_failure()  # 超过限制，返回False
        return self.throttle_success()  # 正常访问，返回True

    def throttle_success(self):
        # 将本次请求事件加入历史访问记录中
        self.history.insert(0, self.now)
        self.cache.set(self.key, self.history, self.duration)
        return True

    def throttle_failure(self):
        return False

    def wait(self):
        if self.history:
            remaining_duration = self.duration - (self.now - self.history[-1])  # 还需要等待的时间
        else:
            remaining_duration = self.duration

        available_requests = self.num_requests - len(self.history) + 1
        if available_requests <= 0:
            return None

        return remaining_duration / float(available_requests)


class MyThrottle(SimpleRateThrottle):
    scope = "XXX"
    THROTTLE_RATES = {"XXX": "5/m"}  # 限制频率
    cache = default_cache  # redis配置

    # 获取唯一标识，后续可以连接redis做操作
    def get_cache_key(self, request, view):
        if request.user:
            ident = request.user.pk  # 用户ID
        else:
            ident = self.get_ident(request)  # 获取请求用户IP（去request中找请求头）
        # cache_format = 'throttle_%(scope)s_%(ident)s'
        return self.cache_format % {'scope': self.scope, 'ident': ident}


class APIView(View):
    throttle_classes = api_settings.DEFAULT_THROTTLE_CLASSES

    def get_throttles(self):
        # 创建限流类实例对象，返回限流类对象列表，局部配置优先于全局
        return [throttle() for throttle in self.throttle_classes]

    def check_throttles(self, request):
        throttle_durations = []
        # 循环每个对象  ->  allow_request到底如何实现的
        for throttle in self.get_throttles():
            # 超过频率限制，不允许访问
            if not throttle.allow_request(request, self):
                # 将等待时间添加进列表
                throttle_durations.append(throttle.wait())

        if throttle_durations:
            durations = [
                duration for duration in throttle_durations
                if duration is not None
            ]
            # 获取最大等待时间
            duration = max(durations, default=None)
            # 抛出异常
            self.throttled(request, duration)

    def throttled(self, request, wait):
        raise exceptions.Throttled(wait)


    def initial(self, request, *args, **kwargs):
        self.perform_authentication(request)  # 认证组件的过程，循环执行每个authenticate方法；失败则抛出异常不向下执行
        self.check_permissions(request)  # 权限的校验
        self.check_throttles(request)

    def dispatch(self, request, *args, **kwargs):
        # 封装drf的request
        request = self.initialize_request(request, *args, **kwargs)
        self.request = request
        self.headers = self.default_response_headers  # deprecate?

        try:
            self.initial(request, *args, **kwargs)
            # 反射获取get/post等方法
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
            # 执行相应方法
            response = handler(request, *args, **kwargs)
        # 异常处理
        except Exception as exc:
            response = self.handle_exception(exc)
        self.response = self.finalize_response(request, response, *args, **kwargs)
        return self.response

class MyThrottle(SimpleRateThrottle):
    scope = "XXX"
    THROTTLE_RATES = {"XXX": "5/m"}  # 限制频率
    cache = default_cache  # redis配置

    # 获取唯一标识，后续可以连接redis做操作
    def get_cache_key(self, request, view):
        if request.user:
            ident = request.user.pk  # 用户ID
        else:
            ident = self.get_ident(request)  # 获取请求用户IP（去request中找请求头）
        # cache_format = 'throttle_%(scope)s_%(ident)s'
        return self.cache_format % {'scope': self.scope, 'ident': ident}    


class OrderView(APIView):
    permission_classes = [MyPermission1, MyPermission2, MyPermission3]  # 权限组件的局部配置
    throttle_classes = []

    def get(self, request):
        print(request.user, request.auth)
        return Response({"status": True, "data": [11, 22, 33, 44]})
```



### 7.4 案例-限流的应用

在前几个组件案例的基础上实现，下列非完整代码，主要是限流功能的实现

> - 无需登录，限流 10/m
> - 登录，限流 5/m

- `settings.py`

```python
REST_FRAMEWORK = {
    "UNAUTHENTICATED_USER": None,  # 未登录时，request.user=None
    "UNAUTHENTICATED_TOKEN": None,  # 未登录时，request.auth=None
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "ext.auth.QueryParamsAuthentication",
        "ext.auth.HeaderAuthentication",
        "ext.auth.NoAuthentication",
    ],
    "DEFAULT_PERMISSION_CLASSES": [
    ],
    "DEFAULT_THROTTLE_RATES": {
        "Ip": "10/m",
        "user": "5/m",
    }
}

```

- `ext/throttle.py`

```python
from rest_framework.throttling import SimpleRateThrottle
from django.core.cache import cache as default_cache


class IpThrottle(SimpleRateThrottle):
    scope = "Ip"
    cache = default_cache  # redis配置

    # 确定是匿名的，因此直接获取ip做标识即可
    def get_cache_key(self, request, view):
        ident = self.get_ident(request)  # 获取请求用户IP（去request中找请求头）
        # cache_format = 'throttle_%(scope)s_%(ident)s'
        return self.cache_format % {'scope': self.scope, 'ident': ident}


class UserThrottle(SimpleRateThrottle):
    scope = "user"
    cache = default_cache  # redis配置

    def get_cache_key(self, request, view):
        ident = request.user.pk  # 用户ID
        return self.cache_format % {'scope': self.scope, 'ident': ident}

```

- `api/views.py`

```python
class LoginView(APIView):
    authentication_classes = []
    throttle_classes = [IpThrottle, ]
	...
    
class AvatarView(NbApiView):
    # 总监、员工可以访问
    permission_classes = [BossPermission, UserPermission, ]
    throttle_classes = [UserThrottle, ]
    ...
```



### 7.5 总结

全局配置限流频率`settings.py`

```python
REST_FRAMEWORK = {
    "UNAUTHENTICATED_USER": None,  # 未登录时，request.user=None
    "UNAUTHENTICATED_TOKEN": None,  # 未登录时，request.auth=None
    "DEFAULT_AUTHENTICATION_CLASSES": [
        "ext.auth.QueryParamsAuthentication",
        "ext.auth.HeaderAuthentication",
        "ext.auth.NoAuthentication",
    ],
    "DEFAULT_PERMISSION_CLASSES": [
        "ext.per.Mypermission",
    ],
    "DEFAULT_THROTTLE_RATES": {
        "Ip": "10/m",
        "user": "5/m",
    }
}
```

限流组件的定义`ext/throttle.py`，局部配置可覆盖全局定义的频率

```python
from rest_framework.throttling import SimpleRateThrottle
from django.core.cache import cache as default_cache


class IpThrottle(SimpleRateThrottle):
    # 局部配置
    scope = "Ip"
    THROTTLE_RATES = {"Ip": "5/m"}
    cache = default_cache  # redis配置

    # 确定是匿名的，因此直接获取ip做标识即可
    def get_cache_key(self, request, view):
        ident = self.get_ident(request)  # 获取请求用户IP（去request中找请求头）
        # cache_format = 'throttle_%(scope)s_%(ident)s'
        return self.cache_format % {'scope': self.scope, 'ident': ident}


class UserThrottle(SimpleRateThrottle):
    # 采用全局
    scope = "user"
    cache = default_cache  # redis配置

    def get_cache_key(self, request, view):
        ident = request.user.pk  # 用户ID
        return self.cache_format % {'scope': self.scope, 'ident': ident}

```

视图函数应用限流类

```python
from ext.throttle import MyThrottle
class LoginView(APIView):
    throttle_classes = [MyThrottle, ]
	...
```



## 8. 版本

> API版本控制是一个关键的实践，它有助于确保API的稳定性、可靠性和兼容性，同时为API的长期维护和演进提供支持。通过在请求中携带版本号，开发者可以更好地管理API的不同版本，并确保对客户端的影响最小化

### 8.1 GET参数传递

> 配置文件中的`VERSION_PARAM`决定哪个参数表示版本信息
>
> 参数携带的版本信息会被封装到`request.version`

![image-20241219203724421](post\python\DRF\image-20241219203724421.png)

- `settings.py`

```python
REST_FRAMEWORK = {
    # 携带版本信息的请求参数名
    'VERSION_PARAM': 'version',
    # 未传入版本参数时的默认版本
    'DEFAULT_VERSION': "v1",
    # 版本范围列表
    'ALLOWED_VERSIONS': ["v1", "v2"],
    # 默认版本类
    'DEFAULT_VERSIONING_CLASS': 'rest_framework.versioning.QueryParameterVersioning',
}
```



### 8.2 反向生成URL

> 与django中的反向生成URL不同，drf反向生成的URL可以携带相关版本信息

![image-20241220092026291](post\python\DRF\image-20241220092026291.png)

GET请求携带的其他参数不会自动生成

### 8.3 源码解读

```
1 入口在APIView类的initial()方法，调用determine_version方法，将版本信息和版本类封装到request中version，versioning_scheme

2 determine_version方法，如果没有定义版本类，返回(None, None)，定义了版本类，将其实例化赋值给versioning_scheme，调用scheme的determine_version，返回结果赋值version。
	2.1 版本类实例化时，继承父类，读取全局配置default_version，allowed_versions，version_param，实例化后赋值给versioning_scheme
	2.2 调用scheme的determine_version，从请求参数中读取版本信息version，并且调用is_allowed_version方法，判断version值是否合法：allowed_versions为空或version是默认值或在allowed_versions范围内，合法：返回给version

当视图函数中要反向生成url时，request.versioning_scheme.reverse("hm",request=request)
1 调用版本类的reverse方法，并传入路由的别名
	1.1 调用父类的reverse，传入路由别名，反向生成url（就是django中的）
	1.2 调用replace_query_param，通过query_dict和urlencode将版本信息插入到url中
```



```python
class BaseVersioning:
    default_version = api_settings.DEFAULT_VERSION
    allowed_versions = api_settings.ALLOWED_VERSIONS
    version_param = api_settings.VERSION_PARAM

    def determine_version(self, request, *args, **kwargs):
        msg = '{cls}.determine_version() must be implemented.'
        raise NotImplementedError(msg.format(
            cls=self.__class__.__name__
        ))

    def reverse(self, viewname, args=None, kwargs=None, request=None, format=None, **extra):
        return _reverse(viewname, args, kwargs, request, format, **extra)

    def is_allowed_version(self, version):
        # 列表为空 -> 所有版本均可
        if not self.allowed_versions:
            return True
        # version是默认值或者在allowed_versions列表范围内
        return ((version is not None and version == self.default_version) or
                (version in self.allowed_versions))


class QueryParameterVersioning(BaseVersioning):
    """
    GET /something/?version=0.1 HTTP/1.1
    Host: example.com
    Accept: application/json
    """
    invalid_version_message = _('Invalid version in query parameter.')

    def determine_version(self, request, *args, **kwargs):
        # 去请求参数中获取键为version_param的值即为版本，若不存在则读取settings.py中的默认值
        version = request.query_params.get(self.version_param, self.default_version)
        if not self.is_allowed_version(version):
            raise exceptions.NotFound(self.invalid_version_message)
        return version

    # 传入视图名称和request
    def reverse(self, viewname, args=None, kwargs=None, request=None, format=None, **extra):
        # 本质上就是调用django的reverse反向生成URL
        url = super().reverse(
            viewname, args, kwargs, request, format, **extra
        )
        # 将版本信息添加或替换进去
        if request.version is not None:
            return replace_query_param(url, self.version_param, request.version)
        return url


def replace_query_param(url, key, val):
    (scheme, netloc, path, query, fragment) = parse.urlsplit(force_str(url))
    query_dict = parse.parse_qs(query, keep_blank_values=True)
    query_dict[force_str(key)] = [force_str(val)]
    query = parse.urlencode(sorted(query_dict.items()), doseq=True)
    return parse.urlunsplit((scheme, netloc, path, query, fragment))


class APIView(View):
    versioning_class = api_settings.DEFAULT_VERSIONING_CLASS

    def determine_version(self, request, *args, **kwargs):
        if self.versioning_class is None:
            return (None, None)
        # 实例化类对象，即QueryParameterVersioning(),，欸有__init__方法
        scheme = self.versioning_class()
        # 返回版本信息与版本类对象
        return (scheme.determine_version(request, *args, **kwargs), scheme)

    def initial(self, request, *args, **kwargs):
        # 将版本信息与版本类封装到request中
        version, scheme = self.determine_version(request, *args, **kwargs)
        request.version, request.versioning_scheme = version, scheme

        # Ensure that the incoming request is permitted
        self.perform_authentication(request)
        self.check_permissions(request)
        self.check_throttles(request)

    def dispatch(self, request, *args, **kwargs):
        self.args = args
        self.kwargs = kwargs
        request = self.initialize_request(request, *args, **kwargs)
        self.request = request
        self.headers = self.default_response_headers  # deprecate?

        try:
            self.initial(request, *args, **kwargs)
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
            response = handler(request, *args, **kwargs)
        except Exception as exc:
            response = self.handle_exception(exc)

        self.response = self.finalize_response(request, response, *args, **kwargs)
        return self.response


class HomeView(APIView):
    # 配置文件 VERSION_PARAM
    # http://127.0.0.1:8000/home/?xx=123&page=44&vertion=v1     ->    request.version
    versioning_class = QueryParameterVersioning

    def get(self, request):
        print(request.version)
        return Response("...")

```

### 8.4 URL路径传递

> urls.py中path和re_path两种方式均可

![image-20241220114016697](post\python\DRF\image-20241220114016697.png)

```python
# 源码
class URLPathVersioning(BaseVersioning):    
    def determine_version(self, request, *args, **kwargs):
        version = kwargs.get(self.version_param, self.default_version)
        if version is None:
            version = self.default_version

        if not self.is_allowed_version(version):
            raise exceptions.NotFound(self.invalid_version_message)
        return version
```

> 从`kwargs`中读取版本，因此视图函数要用`*args`和`**kwargs`接收，其他源码逻辑与GET参数传递基本相同
>
> URL参数传递不需要`*args`和`**kwargs`接收是因为传参可以从request中可以取出



### 8.5 Accept请求头传递

> 反向生成的URL不含版本，因为他本来就不出现在url中

![image-20241220121052593](post\python\DRF\image-20241220121052593.png)

**参考代码**

```python
# urls.py
from django.urls import path, re_path
from api import views

urlpatterns = [
    # path('admin/', admin.site.urls),
    path('home/', views.HomeView.as_view(), name='hm'),
    # path('api/<str:version>/home/', views.HomeView.as_view(), name='hm'),
    # re_path(r'^api/(?P<version>\w+)/home/$', views.HomeView.as_view(), name='hm'),
]
```

```python
# views.py
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.versioning import QueryParameterVersioning, URLPathVersioning, AcceptHeaderVersioning


# Create your views here.
class HomeView(APIView):
    # 配置文件 VERSION_PARAM
    # http://127.0.0.1:8000/home/?xx=123&page=44&vertion=v1     ->    request.version
    versioning_class = AcceptHeaderVersioning

    def get(self, request, *args, **kwargs):
        print(request.version)
        print(request.versioning_scheme)
        url = request.versioning_scheme.reverse('hm', request=request)
        print("反向生成的url:", url)
        return Response("...")

```

### 8.6 总结

> 后续开发时，一般都采用URL路径传递的形式，下列给出开发示例

- `settings.py进行全局配置`

```python
REST_FRAMEWORK = {
    'UNAUTHENTICATED_USER': None,
    'VERSION_PARAM': 'version',
    'DEFAULT_VERSION': "v1",
    'ALLOWED_VERSIONS': ["v1", "v2"],
    # 全局配置
    'DEFAULT_VERSIONING_CLASS': 'rest_framework.versioning.URLPathVersioning',
}
```

- `views.py`

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.versioning import QueryParameterVersioning, URLPathVersioning, AcceptHeaderVersioning

class HomeView(APIView):
    # 局部配置
    versioning_class = URLPathVersioning
    def get(self, request, *args, **kwargs):
        print(request.version)
        print(request.versioning_scheme)
        # 反向生成url
        url = request.versioning_scheme.reverse('hm', request=request)
        print("反向生成的url:", url)
        return Response("...")
```

- `urls.py`

```python
from django.urls import path, re_path
from api import views

urlpatterns = [
    path('api/<str:version>/home/', views.HomeView.as_view(), name='hm'),
]
```



## 9. 解析器

在 `Django REST Framework（DRF）`中，解析器（Parser）是用于处理传入请求数据的组件。解析器负责将原始请求数据`request.body`（如 JSON、表单数据、XML 等）解析成 Python 数据结构，使得视图函数可以方便地访问和操作请求数据，让序列化器可以进一步处理这些数据

### 9.1 流程概述

解析器一般用于解析POST请求请求体中的数据，与内容协商机制（Content Negotiation）配合，`content-type`不同，客户端传递来的数据格式也不同，根据客户端的 `Content-Type` 头部选择合适的解析器来解析请求体

大体的流程为：

1. 读取请求头
2. 根据请求头解析数据
   - 根据请求头获取解析器 -> 如JSON解析器
   - request.data = JSON解析器.parse
3. request.data获取处理后的数据

```python
class Form 解析器

	content-type:"urlencode"

class JSON 解析器

	content-type:"application/json"
	
    def parse():
    	...
        
请求者：
	GET
    	http://127.0.0.1:8000/api/home/?xxx=123&abc=xxx   -->  request.query_params
        请求头
    POST
    	http://127.0.0.1:8000/api/home/
        请求头
        	content-type:"urlencode..."
    		content-type:"application/json"
        请求体
        	name=zhangs&age=18
            {"name":"zhangs","age":19}
```



### 9.2 常见应用

#### 9.2.1 JSONParser

解析`JSON`格式的请求数据（`application/json`），返回python中的`字典`类型

![image-20241221095817376](post\python\DRF\image-20241221095817376.png)

#### 9.2.2 FormParser

解析`表单`（`application/x-www-form-urlencoded`）数据为`QueryDict（字典）`类型数据

![image-20241221100042471](post\python\DRF\image-20241221100042471.png)



#### 9.2.3 MultiPartParser

解析`多部分表单数据`（`multipart/form-data`），支持同时解析文件和表单字段，通常用于文件上传

![image-20241221100114196](post\python\DRF\image-20241221100114196.png)



#### 9.2.4 FileUploadParser

只能处理`文件`上传请求`application/octet-stream`

![image-20241221100143428](post\python\DRF\image-20241221100143428.png)



### 9.3 源码流程

```
dispatch()的流程走完仅仅实现parsers，negotiator，parser_context等值的封装，真正的解析发生在调用request.data时

加载时触发dispatch()方法，调用initialize_request
	调用get_parser_context方法，将视图对象，URL路由参数封装成对象赋值给parser_context
	调用get_parsers，循环解析器类对象列表parser_classes创建实例列表parsers
	调用get_content_negotiator，将协商器content_negotiation_class实例化后的对象赋值给negotiator
	将parser_context，parsers，negotiator一并封装进Request中
	在Request的init方法中：往parser_context中进一步添加了request和encoding数据
	
initialize_request执行完，调用initial方法
	调用perform_content_negotiation方法，将被选择的渲染器实例赋值给accepted_renderer，将对应的媒体类型字符串赋值给accepted_media_type
		循环实例化获取渲染器实例化对象列表renderers，conneg为协商器对象，调用conneg的select_renderer方法，根据客户端的 Accept 请求头和渲染器的优先级选择一个合适的渲染器
		
当调用request.data时，才真正地解析数据
	如果_full_data有值，直接返回，无值，调用_load_data_and_files加载值
	调用_parse，
		循环解析器列表parsers，调用协商器的select_parser方法，将请求中的content_type与解析器内部的media_type匹配，找到匹配的解析器parser
		调用解析器的parse方法，将数据解析成目标格式，赋值给parsed，返回(parsed.data, parsed.files)
	用_data和_files接收解析后的数据，处理成完整的数据赋值给_full_data，下次调用request.data可以直接返回
	
```



```python
class DefaultContentNegotiation(BaseContentNegotiation):
    # 根据请求头类型与解析器内定义的media_type匹配出对应的解析器
    def select_parser(self, request, parsers):
        for parser in parsers:
            if media_type_matches(parser.media_type, request.content_type):
                return parser
        return None


class FormParser(BaseParser):
    media_type = 'application/x-www-form-urlencoded'

    def parse(self, stream, media_type=None, parser_context=None):
        parser_context = parser_context or {}
        encoding = parser_context.get('encoding', settings.DEFAULT_CHARSET)
        return QueryDict(stream.read(), encoding=encoding)


class JSONParser(BaseParser):
    media_type = 'application/json'
    renderer_class = renderers.JSONRenderer
    strict = api_settings.STRICT_JSON

    def parse(self, stream, media_type=None, parser_context=None):
        parser_context = parser_context or {}
        encoding = parser_context.get('encoding', settings.DEFAULT_CHARSET)

        try:
            decoded_stream = codecs.getreader(encoding)(stream)
            parse_constant = json.strict_constant if self.strict else None
            return json.load(decoded_stream, parse_constant=parse_constant)
        except ValueError as exc:
            raise ParseError('JSON parse error - %s' % str(exc))


class Request:
    def __init__(self, request, parsers=None, authenticators=None, negotiator=None, parser_context=None):
        self._request = request
        self.parsers = parsers or ()
        self.authenticators = authenticators or ()
        self.negotiator = negotiator or self._default_negotiator()
        self.parser_context = parser_context
        self._data = Empty
        self._files = Empty
        self._full_data = Empty
        self._content_type = Empty
        self._stream = Empty

        if self.parser_context is None:
            self.parser_context = {}
        self.parser_context['request'] = self
        self.parser_context['encoding'] = request.encoding or settings.DEFAULT_CHARSET

    @property
    def data(self):
        if not _hasattr(self, '_full_data'):
            # 加载值
            self._load_data_and_files()
        return self._full_data

    def _load_data_and_files(self):
        if not _hasattr(self, '_data'):
            self._data, self._files = self._parse()
            if self._files:
                self._full_data = self._data.copy()
                self._full_data.update(self._files)
            else:
                self._full_data = self._data

            if is_form_media_type(self.content_type):
                self._request._post = self.POST
                self._request._files = self.FILES

    @property
    def content_type(self):
        meta = self._request.META
        return meta.get('CONTENT_TYPE', meta.get('HTTP_CONTENT_TYPE', ''))

    def _parse(self):
        # 获取请求头中的content_type
        media_type = self.content_type
        # 请求发送过来的原始数据
        stream = self.stream
        # DefaultContentNegotiation.select_parser()
        # 循环解析器列表，找到content-type符合的解析器并返回
        parser = self.negotiator.select_parser(self, self.parsers)  # request [JSONParserm,FormParser]
        # 执行解析器的parse方法
        parsed = parser.parse(stream, media_type, self.parser_context)
        try:
            return (parsed.data, parsed.files)
        except AttributeError:
            empty_files = MultiValueDict()
            return (parsed, empty_files)


class APIView(View):
    renderer_classes = api_settings.DEFAULT_RENDERER_CLASSES
    parser_classes = api_settings.DEFAULT_PARSER_CLASSES
    content_negotiation_class = api_settings.DEFAULT_CONTENT_NEGOTIATION_CLASS

    def get_parser_context(self, http_request):
        return {
            'view': self,
            'args': getattr(self, 'args', ()),
            'kwargs': getattr(self, 'kwargs', {})
        }

    def get_parsers(self):
        return [parser() for parser in self.parser_classes]

    def get_content_negotiator(self):
        if not getattr(self, '_negotiator', None):
            self._negotiator = self.content_negotiation_class()
        return self._negotiator

    def initialize_request(self, request, *args, **kwargs):
        # {视图对象，URL路由参数}
        parser_context = self.get_parser_context(request)

        return Request(
            request,  # django的request
            parsers=self.get_parsers(),  # 解析器 [JSONParser(),FormParser(),]
            authenticators=self.get_authenticators(),  # 认证组件 [认证对象1，认证对象2]
            negotiator=self.get_content_negotiator(),  # DefaultContentNegotiation()
            parser_context=parser_context  # {视图对象，URL路由参数 + drf的request,encoding}
        )

    def dispatch(self, request, *args, **kwargs):
        self.args = args
        self.kwargs = kwargs
        request = self.initialize_request(request, *args, **kwargs)
        self.request = request
        self.headers = self.default_response_headers  # deprecate?

        try:
            self.initial(request, *args, **kwargs)
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
            response = handler(request, *args, **kwargs)
        except Exception as exc:
            response = self.handle_exception(exc)
        self.response = self.finalize_response(request, response, *args, **kwargs)
        return self.response

    def initial(self, request, *args, **kwargs):
        neg = self.perform_content_negotiation(request)
        # 第一个元素是被选择的渲染器实例，第二个元素是对应的媒体类型字符串
        request.accepted_renderer, request.accepted_media_type = neg

        version, scheme = self.determine_version(request, *args, **kwargs)
        request.version, request.versioning_scheme = version, scheme

        # Ensure that the incoming request is permitted
        self.perform_authentication(request)
        self.check_permissions(request)
        self.check_throttles(request)
	# 该方法负责根据客户端的 Accept 请求头来确定响应的内容类型。这个方法会检查客户端接受哪些媒体类型，并根据服务器能够提供的媒体类型来选择一个合适的渲染器（Renderer）
    def perform_content_negotiation(self, request, force=False):
        renderers = self.get_renderers()
        conneg = self.get_content_negotiator()

        try:
            return conneg.select_renderer(request, renderers, self.format_kwarg)
        except Exception:
            if force:
                return (renderers[0], renderers[0].media_type)
            raise

    def get_renderers(self):
        return [renderer() for renderer in self.renderer_classes]

    def get_content_negotiator(self):
        if not getattr(self, '_negotiator', None):
            self._negotiator = self.content_negotiation_class()
        return self._negotiator


class HomeView(APIView):
    # 所有的解析器
    parser_classes = [JSONParser, FormParser]
    # 根据请求，匹配对应的解析器
    content_negotiation = DefaultContentNegotiation

    def post(self, request, *args, **kwargs):
        print(request.data, type(request.data))
        return Response("请求来了")
```

### 9.4 解析器全局配置

如图，默认解析器有`JSONParser,FormParser,MutiPartParser`三种

![image-20241221100502285](post\python\DRF\image-20241221100502285.png)

但为了防止不能上传图片等功能点因为默认解析器可以上传图片，导致`request.data`的类型的不确定性，建议局部配置

修改全局配置

```python
REST_FRAMEWORK = {
    'UNAUTHENTICATED_USER': None,
    'VERSION_PARAM': 'version',
    'DEFAULT_VERSION': "v1",
    'ALLOWED_VERSIONS': ["v1", "v2"],
    'DEFAULT_VERSIONING_CLASS': 'rest_framework.versioning.QueryParameterVersioning',
    
    'DEFAULT_PARSER_CLASSES': ['rest_framework.parsers.JSONParser', ]  # 设置默认解析器只有JSONParser
    'DEFAULT_RENDERER_CLASSES':
    'DEFAULT_CONTENT_NEGOTIATION_CLASS':
}
```

后续如果有上传图片的功能点，再局部配置其解析器即可

```python
parser_classes = [MutiPartParser,]
```

### 9.5 总结

- 解析器负责将客户端发送的请求体解析为 Python 可操作的数据结构。DRF 提供了多种默认解析器，例如 `JSONParser`，`FormParser` 和 `MultiPartParser`
- 渲染器负责将响应数据转换为客户端可读的格式。DRF 提供了多种默认渲染器，例如 `JSONRenderer` 和 `BrowsableAPIRenderer` 
- 内容协商器负责根据客户端的 `Accept` 请求头和服务器支持的渲染器选择合适的渲染器。DRF 默认使用 `DefaultContentNegotiation`

```python
REST_FRAMEWORK = {
    'DEFAULT_PARSER_CLASSES': [
        'rest_framework.parsers.JSONParser',
        'rest_framework.parsers.FormParser',
        'rest_framework.parsers.MultiPartParser',
    ],
    'DEFAULT_RENDERER_CLASSES': [
        'rest_framework.renderers.JSONRenderer',
        'rest_framework.renderers.BrowsableAPIRenderer',
    ]
    'DEFAULT_CONTENT_NEGOTIATION_CLASS': [
        'rest_framework.negotiation.DefaultContentNegotiation',
    ]
}
```



```python
from rest_framework.parsers import JSONParser, FormParser, MultiPartParser
from rest_framework.renderers import JSONRenderer
from rest_framework.negotiation import DefaultContentNegotiation


class MyUploadView(APIView):
	parser_classes = [JSONParser, FormParser, MultiPartParser]
	renderer_classes = [JSONRenderer]
    content_negotiation_class = DefaultContentNegotiation
```



## 10. 元类

在Python中，一切皆对象。类本身也是对象，而**元类就是用来创建类的类**。元类（metaclass）可以干预类的创建，控制类的实例化

普通的类用来定义一般对象的属性和行为，元类用来定义普通的类及其实例对象的属性和行为



### 10.1 类和对象的基础知识

```python
# 类和对象，基于类实例化对象

# 类
class Foo:
    # 类变量
    v1 = 123

    def __init__(self, name):
        # 实例变量
        self.name = name

    # 类方法
    def func(self):
        pass


# 对象
obj1 = Foo('test')
obj2 = Foo('test2')
```

### 10.2 创建类的两种方式

方式一就是我们平常书写的形式，而方式二就显示地展示了元类创建类的过程，本质上方式一经过处理后就是通过方式二创建类的，只是方式一更加直观且易于书写

```python
# 创建类：方式1
class Foo:
    v1 = 123

    def func(self):
        return 999


# 创建类：方式2
# 类名 = type("类名",(父类,),{成员})
# Foo = type("Foo", (object,), {"v1": 123, "func": lambda self: 999})

obj1 = Foo()
print(obj1.v1)
print(obj1.func())

```

### 10.3 MyType创建类与拓展

使用元类，最常见方式是自定义一个元类，继承type，然后使用自定义的元类创建类

```python
class MyType(type):
    def __new__(cls, *args, **kwargs):
        xx = super().__new__(cls, *args, **kwargs)
        print("创建类", xx)
        return xx


Foo = MyType("Foo", (object,), {"v1": 123, "func": lambda self: 999})

obj1 = Foo()
print(obj1.v1)
print(obj1.func())

# 创建类 <class '__main__.Foo'>
# 123
# 999
```

应用在我们常用的类的创建方式时，就是要将元类指定为类的`metaclass`关键字参数，告诉Python在创建类时使用指定的元类

```python
# 基于type创建
# class Foo:
#     v1 = 123
#
#     def func(self):
#         pass

# 基于MyType创建
class MyType(type):
    def __new__(cls, *args, **kwargs):
        xx = super().__new__(cls, *args, **kwargs)
        print("创建类", xx)
        return xx


class Foo(object, metaclass=MyType):
    v1 = 123

    def func(self):
        pass


print(Foo)
```

元类中可以重写`__new__`和`__init__`方法来控制类的创建和初始化过程。`__new__`方法在类创建之前调用，`__init__`方法在类创建之后调用。

`__new__`传入的name, bases, attrs其实就是我们使用方式二创建时传入的类名，父类及及属性，因此就可以在调用type的`__new__`创建类之前，对传入的name, bases, attrs进行修改，从而干预类的创建

```python
class MyType(type):
    def __new__(cls, name, bases, attrs):
        # Foo (<class 'object'>,) {'__module__': '__main__', '__qualname__': 'Foo', 'v1': 123, 'func': <function Foo.func at 0x000002209BA9C310>}
        print(name, bases, attrs)
        # 可以在创建空间前对类进行更改，如添加/删除属性等等
        del attrs['v1']
        attrs['add'] = 'xxx'
        xx = super().__new__(cls, name, bases, attrs)
        return xx


class Foo(object, metaclass=MyType):
    v1 = 123

    def func(self):
        pass


print(Foo.add)
print(Foo.v1)

```

### 10.4 继承关系

指定`metaclass`的类以及他所有的子类都将由指定的`metaclass`创建

```python
class MyType(type):
    def __new__(cls, name, bases, attrs):
        print(name, bases, attrs)
        xx = super().__new__(cls, name, bases, attrs)
        return xx


class Info(object):
    pass


class Foo(object, metaclass=MyType):
    pass


class Bar(Foo):
    pass

Info()
Foo()
Bar()
```

![image-20241221113024024](post\python\DRF\image-20241221113024024.png)



### 10.5 drf序列化源码案例

因为UserSerializer是Serializer的子类，而Serializer指定了metaclass=SerializerMetaclass，因此由上一小节的继承关系的介绍可知，UserSerializer也将由元类SerializerMetaclass创建，因此他的实例空间中有_declared_fields属性

```python
class SerializerMetaclass(type):
    def __new__(cls, name, bases, attrs):
        data_dict = {}
        for k, v in list(attrs.items()):  # {"v1":123,"v2":123,"v3":123}
            if isinstance(v, int):
                data_dict[k] = attrs.pop(k)
        attrs['_declared_fields'] = data_dict
        return super().__new__(cls, name, bases, attrs)


class BaseSerializer(object):
    pass


class Serializer(BaseSerializer, metaclass=SerializerMetaclass):
    pass


class ModelSerializer(Serializer):
    pass


class UserSerializer(ModelSerializer):
    v1 = 123
    v2 = 456
    v3 = "哈哈哈"


print(UserSerializer.v3)
print(UserSerializer._declared_fields)
```

运行结果：

```python
哈哈哈
{'v1': 123, 'v2': 456}
```



### 10.6 扩展

类是由MyType创建出来，所以类其实是MyType类实例化的对象。

Base是类，也是MyType类的对象即Mytype()；  Base()是Base类实例化的对象，相当于MyType()()

- 因此Base类实例化的对象就会执行MyType类的`__call__`
- 而obj()便会执行Base类的`__call__`

```python
class MyType(type):
    def __new__(cls, name, bases, attrs):
        print(name, bases, attrs)
        xx = super().__new__(cls, name, bases, attrs)
        return xx

    def __call__(cls, *args, **kwargs):
        print("执行type的call")
        obj = cls.__new__(cls, *args, **kwargs)
        print("----")
        cls.__init__(obj, *args, **kwargs)
        return obj


class Base(object, metaclass=MyType):
    def __init__(self):
        print("初始化")

    def __new__(cls, *args, **kwargs):
        print("实例化类的对象")
        return object.__new__(cls)

    def __call__(self, *args, **kwargs):
        print("Base.call")


obj = Base()  # 执行MyType的__call__
obj()  # 执行Base的__call__
```



## 11. 序列化器



序列化器是Django框架中的一个重要概念，用于在Python对象和JSON等格式之间进行相互转换。通过序列化器，我们可以方便地将模型实例转换为JSON格式的数据，并且还可以对数据进行验证、创建和更新等操作。

- 解析器：
  - 将客户端发送的请求体（如JSON）解析为 Python 数据结构（如字典或列表）
  - 解析后的数据存储在 `request.data` 中，供视图和序列化器使用
- 序列化器：
  - 在视图中，`request.data` 被传递给序列化器，验证解析后的数据是否符合模型字段的要求
  - 将验证后的数据转换为模型实例，保存到数据库
  - 响应阶段，序列化器将模型实例序列化为Python 数据结构（通常是字典，列表，有序字典）
- 渲染器：
  - 将Python 数据结构（通常是字典，列表，有序字典）进一步转换为JSON、XML 或其他格式
  - 将序列化后的数据转换为字节流，最终返回给客户端



### 11.1 序列化

序列化过程 是将从ORM中获取的复杂的数据类型（如 Django 模型实例或查询集）转换为 Python 数据结构（通常是字典，列表，有序字典），然后进一步转换为 JSON、XML 或其他格式的过程（渲染器作用）



#### 11.1.1 Serializer

`Serializer`是基础序列化器，需要手动定义每个字段

先创建数据库，以便后续测试获取数据

![image-20241221182140560](post\python\DRF\image-20241221182140560.png)

手动添加两组数据

![image-20241221182452157](post\python\DRF\image-20241221182452157.png)

模型实例对象

![image-20241221184453282](post\python\DRF\image-20241221184453282.png)

```python
class DepartSerializer(serializers.Serializer):
    title = serializers.CharField()
    count = serializers.IntegerField()


class DepartView(APIView):
    def get(self, request, *args, **kwargs):
        # 1.数据库中获取数据 obj.title  obj.order  obj.count
        depart_object = models.Depart.objects.all().first()  # 模型实例

        # 2.序列化器转换JSON格式
        ser = DepartSerializer(instance=depart_object)
        # {'title': '技术部', 'count': 10}
        print(ser.data)  # 字典

        # 返回给用户
        context = {"status": True, "data": ser.data}  # 字典
        return Response(context)  # Response对象通过渲染器将字典对象zhuan转换为JSON对象
```

Queryset数据集对象

![image-20241221185028899](post\python\DRF\image-20241221185028899.png)

#### 11.1.2 ModelSerializer

`ModelSerializer`是`Serializer`的子类，专门为模型数据设计，提供了基于模型类自动生成字段的功能

如下图，指定model为Depart类并指定fields即可基于模型类Depart的字段自动生成字段

![image-20241221185924204](post\python\DRF\image-20241221185924204.png)

```python
class DepartSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.Depart
        fields = '__all__'
```

#### 11.1.3 字段和参数

上面我们已经学习了`Serializer`和`ModelSerializer`两种序列化器，但是仅仅如此远远达不到我们开发时需要的灵活的，因此，这一部分将介绍对字段及参数的自定义，来实现灵活开发

前置数据准备

![image-20241221194402180](post\python\DRF\image-20241221194402180-17347814432471.png)

![image-20241221194618320](post\python\DRF\image-20241221194618320.png)

##### 11.1.3.1 返回所有字段

返回模型类中定义的所有字段

![image-20241221195330516](post\python\DRF\image-20241221195330516.png)

```python
class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.UserInfo
        fields = '__all__'


class UserView(APIView):
    def get(self, request, *args, **kwargs):
        models.UserInfo.objects.all().update(ctime=datetime.datetime.now())
        # 1.获取数据
        queryset = models.UserInfo.objects.all()
        # 2.序列化
        ser = UserSerializer(queryset, many=True)
        # 3.返回给用户
        context = {"status": True, "data": ser.data}
        return Response(context)
```

##### 11.1.3.2 指定字段返回

有时我们不需要将类中定义的全部字段返回，于是就需要自定义字段返回

![image-20241222160835246](post\python\DRF\image-20241222160835246.png)

我们发现gender返回的是数据库中真实存储的1和2，那应该如何让他返回男和女呢？

```python
# 我们可以自定义字段
gender = serializers.CharField(source='gender')  # 等同于直接在fields中添加 'gender'，拿到的还是1和2
# 它拿到每一个对象都会.source ，上述语句即obj.gender即可拿到数据库中的值，那么知道了这个，我们就可以自定义字段指定其source来拿到我们想要的数据
```

##### 11.1.3.3 子定义字段返回

- `choices展示`
  - `obj.get_gender_display`即可获取choices中的男或女

- `foreignkey跨表查询`
  - 通过FK进行跨表查询

- `时间数据格式设置`



![image-20241222161854882](post\python\DRF\image-20241222161854882.png)

```python
class UserSerializer(serializers.ModelSerializer):
    gender_text = serializers.CharField(source='get_gender_display')
    depart = serializers.CharField(source='depart.title')
    ctime = serializers.DateTimeField(format='%Y-%m-%d')

    class Meta:
        model = models.UserInfo
        # fields = '__all__'
        fields = ['name', 'age', 'gender', 'gender_text', 'depart', 'ctime']
```

我们已经能做到获取数据库中任何我们想要的数据，那我们可以添加非数据库字段吗？

##### 11.1.3.4 自定义方法

```python
xxx = serializers.SerializerMethodField()
# 当定义了一个名为xxx的SerializerMethodField字段时，会寻找get_xxx方法，将其返回值作为该字段的值
def get_xxx(self, obj):
    return "123" + obj.name
# 传入的obj为每次循环的数据库对象，obj.name即可获取对应的数据库内的数据
```

![image-20241222162904065](post\python\DRF\image-20241222162904065.png)

#### 11.1.4 嵌套与继承

老样子，先拓展一下我们的数据表，添加一个标签，让它与用户构建多对多的关系

```python
from django.db import models


# Create your models here.
class Depart(models.Model):
    title = models.CharField(verbose_name="部门", max_length=32)
    order = models.IntegerField(verbose_name="顺序")
    count = models.IntegerField(verbose_name="人数")


class Tag(models.Model):
    caption = models.CharField(verbose_name="标签", max_length=32)


class UserInfo(models.Model):
    name = models.CharField(verbose_name="姓名", max_length=32)
    age = models.IntegerField(verbose_name="年龄")
    gender = models.SmallIntegerField(verbose_name="性别", choices=((1, "男"), (2, "女")))
    depart = models.ForeignKey(verbose_name="部门", to="Depart", on_delete=models.CASCADE)
    ctime = models.DateTimeField(verbose_name="时间", auto_now_add=True)
    tags = models.ManyToManyField(verbose_name="标签", to="Tag")
```

然后创建并应用迁移对象实现对数据库的操作

添加如下数据以便后续测试

![image-20241222191532495](post\python\DRF\image-20241222191532495.png)

##### 11.1.4.1 嵌套

那么需求来了，我们如何返回一个用户对应的所有标签呢？

最大的Queryset对象存放的是一个一个的用户对象obj，`obj.tags.all()`即可获得所有的标签对象[Tag1,Tag2,....]

那么思路有了我们就可以用上面学会的自定义方法实现

![image-20241222192549232](post\python\DRF\image-20241222192549232.png)

```python
    def get_xxx(self, obj):
        queryset = obj.tags.all()
        result = [{"id": tag.id, "caption": tag.caption} for tag in queryset]
        return result
```

我们通过自定义方法可以实现该需求，但仍需要自己书写逻辑，其实DRF的序列化支持嵌套，下面就来看看如何利用嵌套实现

![image-20241222193614643](post\python\DRF\image-20241222193614643.png)

如上图，一个序列化器可以嵌套子序列化器，子序列化器中的操作与上一节介绍的无异，支持自定义字段，自定义方法等等，这样就快捷地实现了我们的需求

```python
class D1(serializers.ModelSerializer):
    class Meta:
        model = models.Depart
        fields = ["id", "title"]


class D2(serializers.ModelSerializer):
    ex = serializers.SerializerMethodField()

    class Meta:
        model = models.Tag
        fields = ["caption", "ex"]

    def get_ex(self, obj):
        return "xxx"


class UserSerializer(serializers.ModelSerializer):
    depart = D1()
    tags = D2(many=True)

    class Meta:
        model = models.UserInfo
        fields = ['name', 'age', "depart", "tags"]
```



总结：一般在有`foreignkey`或者`manytomany`时使用

##### 11.1.4.2 继承

![image-20241222194350380](post\python\DRF\image-20241222194350380.png)

如上图，Base序列化器定义了一个xx字段，而UserSerializer序列化器继承了Base，就可以直接使用xx字段

```python
class Base(serializers.Serializer):
    xx = serializers.CharField(source="name")


class UserSerializer(serializers.ModelSerializer, Base):
    class Meta:
        model = models.UserInfo
        fields = ['name', 'age', "xx"]
```



**至此，序列化数据的实现就已经全部介绍了，经过上面的学习，我们应该能够具备，无论是数据库中任何表结构关系，获取到模型对象或者是QuerySet时，可以将其进行序列化转为JSON数据返回**



### 11.2 源码流程

#### 11.2.1 流程概述

第一步：加载字段，创建类

- ```python
  class BaseSerializer(serializers.Serializer):
      xx = serializers.CharField(source="name")
  ```

- 1. 在类成员中提取出xx字段并删除

- 2. 汇总到 `BaseSerializer._declared_fields = {"xx":对象}`

- ```python
  class UserSerializer(serializers.ModelSerializer, BaseSerializer):
      yy = serializers.CharField(source="name")
      
      class Meta:
          model = models.UserInfo
          fields = ['name', 'age', "yy" ,"xx"]
  ```

- 1. 在类成员中提取出yy字段并删除
  2. 将自己即继承类中的字段都汇总到`UserSerializer._declared_fields = {"yy":对象,"xx":对象,"name":对象...}`

第二步：序列化

- ```python
  queryset = models.UserInfo.objects.all()
  ser = UserSerializer(queryset, many=True)  # ListSerizlizer对象。循环queryset中的每一个对象，对其调用UserSerializer进行实例化
  ```

- `db->queryset = [{id:xxx,name:xxx,age:xxx},{id:xxx,name:xxx,age:xxx},]`

- ```python
  instance = models.UserInfo.objects.all().first()
  ser = UserSerializer(instance, many=False)  # UserSerializer对象
  ```

- `db->instance = {id:xxx,name:xxx,age:xxx}`

- 序列化过程

  - `instance = models.UserInfo.objects.all().first()`
  - `ser = UserSerializer(instance, many=False)`
  - 当执行`ser.data`时触发
    - 内部寻找对应关系
      - 将UserSerializer._declared_fields中的字段与从数据库中的查到的数据的模型类字段一一对应
    - 逐一进行序列化

#### 11.2.2 创建字段对象

首先来思考一个问题

```python
class Foo(object,metaclass=type):
    v1 = 123

    def func(self):
        return 999
```

上述代码的类，在创建时，是先加载字段v1和func还是先创建Foo这个类

如果看不出来，我们不妨换个形式

```python
Foo = type("Foo", (object,), {"v1": 123, "func": lambda self: 999})
```

现在想必很清晰了，一定是先加载完v1,func这两个字段，才能把他们当成参数传递给type去创建Foo类

因此对于下列代码

```python
class InfoSerializer(serializers.Serializer):
    id = serializers.IntegerField()
    title = serializers.CharField()
    order = serializers.IntegerField
```

一定也是先加载id，title，order三个字段再实例化`IntegerField()`对象...

所以我们先来看DRF创建字段对象的源码实现(精简版，用于理解实现逻辑)

```python
class Field:
    _creation_counter = 0

    def __init__(self, ...):
        self._creation_counter = Field._creation_counter
        Field._creation_counter += 1


class IntegerField(Field):
    def __init__(self, **kwargs):
        self.max_value = 111
        super().__init__(**kwargs)
        
        
class CharField(Field):
    def __init__(self, **kwargs):
        self.allow_blank = Flase
        super().__init__(**kwargs)


class InfoSerializer(serializers.Serializer):
    id = serializers.IntegerField()  # {max_value:111, _creation_counter:0}
    title = serializers.CharField()  # {allow_blank:False, _creation_counter:1}
    order = serializers.IntegerField  # {max_value:111, _creation_counter:2}
```

`id = serializers.IntegerField()`在执行时，会实例化一个`IntegerField`对象，先调用自己的`__init__`方法给自己的实例空间写入值，如`max_value`，再调用父类`Field`的`__init__`方法，实现`_creation_counter=0`，同时将`Field`的类变量+1，即`Field._creation_counter = 1`

`title = serializers.CharField()`在执行时，会实例化一个`CharField`对象，逻辑同上，但是他的`_creation_counter=1`，`Field._creation_counter = 2`

`order = serializers.IntegerField`同理，`_creation_counter=2`，`Field._creation_counter = 3`

如果还有其他`XXSerializer`类，他的字段也会接着给自己的`_creation_counter`赋值并且使类变量自增

到这里想必大家都意识到了，Field类相当于维护了一个计数器，给每一个字段赋予了创建的顺序，意义何在？

就是让后续开发时，根据字段创建的顺序，来决定源码处理的顺序(python3.6之前字典是无序的，因此需要这样一个逻辑)

所以想让那个字段先处理，就把他的位置写的上面一些，例如密码`password`与确认密码`confirm_password`就需要这样一个先后关系



#### 11.2.3 创建类

上一小节中我们已经了解了字段对象的创建逻辑，那么在字段加载完成后，字段将会被作为参数传递进去创建`InfoSerializer`类

![image-20241223185239959](post\python\DRF\image-20241223185239959.png)

`InfoSerializer`类的父类是`Serializer`类，该类在创建时指定了`metaclass=SerializerMetaclass`，由元类的知识我们知道，一旦`Serializer`指定了`metaclass=SerializerMetaclass`，那么其所有的子类在创建时都将指定元类为`SerializerMetaclass`，因此不管是继承的`Serializer`还是`ModelSerializer`，最终都由元类`SerializerMetaclass`创建

下面我们就来看看`SerializerMetaclass`是怎么创建类的

```python
class SerializerMetaclass(type):
    @classmethod
    def _get_declared_fields(cls, bases, attrs):
        # 1.1 列表推导式构建自己类的字段对象列表fields
        fields = [(field_name, attrs.pop(field_name))
                  for field_name, obj in list(attrs.items())
                  if isinstance(obj, Field)]
        # 1.2 排序
        fields.sort(key=lambda x: x[1]._creation_counter)

        known = set(attrs)

        def visit(name):
            known.add(name)
            return name

        # 1.3 列表推导式构建父类的字段列表base_fields
        base_fields = [
            # 将未重复的字段和实例添加到base_fields中，同时将字段标记为已添加
            (visit(name), f)
            # 遍历父类，拿到子u但名称f和字段实例f
            for base in bases if hasattr(base, '_declared_fields')
            for name, f in base._declared_fields.items() if name not in known
        ]
        # 1.4 将父类字段与自己的字段合并列表返回
        return OrderedDict(base_fields + fields)

    def __new__(cls, name, bases, attrs):
        # 1.在attrs中新增_declared_fields字段用于存储自己的字段对象和父类的字段对象
        attrs['_declared_fields'] = cls._get_declared_fields(bases, attrs)
        # 2.再调用type的__new__创建类
        return super().__new__(cls, name, bases, attrs)


# 该类在创建时attr一共是五个值，一个整型三个字段对象和一个类
class InfoSerializer(serializers.Serializer):
    v1 = 123
    id = serializers.IntegerField()
    title = serializers.CharField()
    order = serializers.IntegerField

    class Meta:

```

- 1.1 列表推导式构建自己类的字段列表fields

```python
attrs -->  {"v1":123,"id":IntegerField对象,"title":CharField对象,...}
```

循环获取字段名`field_name`和字段值`obj`，如果`obj`是`Field`的子类，则构建元组加入fields列表，同时在attrs中剔除，

因此循环结束后

```python
attrs -->  {"v1":123,"Meta":对象}
fields = [("id",对象),("title",对象),("order",对象)]
```

- 1.2 排序

还记得上一节创建字段对象时的那个计数器吗？这一步的排序便是依据各个字段对象内的`_creation_counter`大小进行排序

- 1.3 列表推导式构建父类的字段列表base_fields

循环父类的`_declared_fields`，将当前类没有的字段添加到列表`base_fields`中

- 1.4 将父类字段与自己的字段合并列表返回

简单的将两个列表相加返回，再调用type的`__new__`实现类的创建



#### 11.2.4 实例化类

创建对象以及前置的加载字段都是在django项目运行时就执行的，而当请求到来时，视图函数从数据库获取数据并序列化，此时就来到了实例化类的过程

```python
queryset = models.UserInfo.objects.all()
ser = UserSerializer(queryset, many=True)

instance = models.UserInfo.objects.all().first()
ser = UserSerializer(instance, many=False)
```

先讲解第二个，即单个对象的情况

实例化类依次执行`BaseSerializer`类的`__new__`，根据many属性的值分为两路，当前情况时调用父类即Field的`__new__`方法，Field又调用object基类的`__new__`创建实例

创建实例后，会调用 `BaseSerializer` 的 `__init__` 方法来初始化实例

根据序列化器类继承的不同，调用`ModelSerializer`或`Serializer`的`get_fields`方法，该处讲解前一个情况，根据模型字段生成序列化字段

调用get_fields() 获取序列化器中定义的所有字段fields

- 显示声明的字段declared_fields
- 根据模型字段转化成序列化字段添加到fields中

调用_init_fields 初始化字段，将字段绑定到序列化器实例上（调用Field类的bind方法）

- field_name
- parent
- source字段
- source_attrs字段

```python
class BaseSerializer(Field):
    def __new__(cls, *args, **kwargs):
        if kwargs.pop('many', False):
            return cls.many_init(*args, **kwargs)
        # many=Flase则走这条逻辑，即只处理单对象，则调用object基类的__new__创建对象，紧接着寻找__init__方法
        return super().__new__(cls, *args, **kwargs)


    def __init__(self, instance=None, data=empty, **kwargs):
        # 存储传入的模型实例或查询集
        self.instance = instance
        # 获取序列化器中定义的所有字段
        self.fields = self.get_fields()
        # 初始化字段，将字段绑定到序列化器实例上（调用Field类的bind方法）
        self._init_fields()
    
    def _init_fields(self):
        for field_name, field in self.fields.items():
            field.bind(field_name, self)


class ModelSerializer(Serializer):
    def get_fields(self):
        # 获取在类定义中显式声明的字段，即前面提到的id,title,order
        declared_fields = copy.deepcopy(self._declared_fields)
        # 获取模型的字段信息，去 UserSerializer 中的 Meta 中找 model 字段，即 model = models.UserInfo
        model = getattr(self.Meta, 'model')
        # 返回一个包含模型字段和关系的详细信息的字典,包含了每个字段的详细信息，例如字段类型、是否是关系字段（如外键、多对多字段等）以及其他元数据
        info = model_meta.get_field_info(model)
        # 根据 declared_fields 和 info 来确定最终的字段名称列表。这个方法可能会根据序列化器的配置和模型的字段信息来排除或包含某些字段
        field_names = self.get_field_names(declared_fields, info)

        fields = OrderedDict()
		# 根据模型字段生成序列化字段
        for field_name in field_names:
            # 根据字段的类型和属性来决定使用哪个序列化器字段类，以及如何配置这个字段
            # 简单来说就是把数据库字段转成序列化器字段类如models.CharField类转成CharField类，然后放入fields中
            field_class, field_kwargs = self.build_field(
                source, info, model, depth
            )
            # Create the serializer field.
            fields[field_name] = field_class(**field_kwargs)

        # Add in any hidden fields.
        fields.update(hidden_fields)

        return fields
    
class Field:    
    def bind(self, field_name, parent):
        # 字段的名称，用于在序列化器中标识字段
        self.field_name = field_name
        # 字段所属的序列化器实例
        self.parent = parent
        # 字段的源名称，用于从模型实例中获取数据，默认情况下，source 与字段名称相同
        if self.source is None:
            self.source = field_name

        if self.source == '*':
            self.source_attrs = []
        else:
            # 如果是外键跨表的形式如depart.title，就会被分割成一个列表
            self.source_attrs = self.source.split('.')
```

返回的`ser`是`UserSerializer`序列化器实例，是一个复杂的对象，包含以下内容

- `instance`：传入的模型实例（`UserInfo` 对象）
- `data`：序列化后的数据（通过访问 `ser.data` 获得）
- `fields`：序列化器中定义的所有字段
  - 每个字段实例都有自己的属性和方法，例如：
    - `field_name`：字段的名称。
    - `parent`：字段所属的序列化器实例。
    - `source`：字段的源名称，用于从模型实例中获取数据。
    - `to_representation`：将字段值转换为可序列化的格式。
    - `to_internal_value`：将输入数据转换为 Python 数据类型。
- `errors`：如果序列化过程中发生错误，存储错误信息

`ser.data` 是序列化后的数据

- 当你访问 `ser.data` 时，序列化器会将模型实例转换为一个 Python 字典
- 这个字典可以被进一步渲染为 JSON 格式，以便返回给客户端



如果是第一个，即多个对象的queryset的情况

```python
class BaseSerializer(Field):
    def __new__(cls, *args, **kwargs):
        # many=True，处理多对象，进入if内部，执行many_init方法
        if kwargs.pop('many', False):
            return cls.many_init(*args, **kwargs)
        return super().__new__(cls, *args, **kwargs)

    def __init__(self, instance=None, data=empty, **kwargs):
        self.instance = instance

    @classmethod
    def many_init(cls, *args, **kwargs):
        child_serializer = cls(*args, **kwargs)  # 实例化当前类（如 UserSerializer）的单个对象序列化器
        # 创建一个字典，将 child_serializer 作为子序列化器传递给 ListSerializer
        list_kwargs = {
            'child': child_serializer,
        }
        meta = getattr(cls, 'Meta', None)
        list_serializer_class = getattr(meta, 'list_serializer_class', ListSerializer)  # Meta中可以定义，默认为ListSerializer
        return list_serializer_class(*args, **list_kwargs)  # 实例化ListSerializer()，并将每一个UserSerializer对象传入

class ListSerializer(BaseSerializer):
    def __init__(self, *args, **kwargs):
        # 子序列化器实例，用于序列化每个对象
        self.child = kwargs.pop('child')
        super().__init__(*args, **kwargs)

```

此时返回的就是不是`UserSerializer`对象了，而是`ListSerializer`对象，其实就是一个列表，列表内元素都是`UserSerializer`对象



#### 11.2.5 序列化过程

上一节我们介绍的实例化类对应的是`ser = UserSerializer(queryset, many=True)`这一行代码，此时只是将数据封装到对象中，真正触发序列化动作的是当`ser.data`

##### 11.2.5.1 UserSerializer类

我们先说当前类，即`UserSerializer`类触发序列化的情况



ser.data --> Serializer中的data -->  BaseSerializer中的data --> self._data = self.to_representation(self.instance)

于是关键就是Serializer的to_representation方法，我们下面来分析：



获取所有的字段fields = Serializer._readable_fields __

- __self._readable_fields读取Serializer.fields的值

循环所有的字段对象for field in fields:

- 去数据库对象中依据字段的信息获取数据attribute = field.get_attribute(instance)

  - bind方法处理过的source，如果是外键跨表的形式如depart.title，就会被分割成一个列表存入self.source_attrs

    Field.get_attribute调用 get_attribute(instance, self.source_attrs)

  - get_attribute循环source_attrs取出instance对应该source的值

- 将获取到的数据通过序列化器类的to_representation方法转变一下格式加入ret中，如IntegerField会对数据进行int()转换等等ret[field.field_name] = field.to_representation(attribute)

- 将ref返回

精简版的源码，结合上述文字流程食用

```python
from django.contrib.gis.gdal.raster import source


class BaseSerializer(Field):
    @property
    def data(self):
        if not hasattr(self, '_data'):
            if self.instance is not None and not getattr(self, '_errors', None):
                # 真正实现序列化的步骤
                self._data = self.to_representation(self.instance)
            elif hasattr(self, '_validated_data') and not getattr(self, '_errors', None):
                self._data = self.to_representation(self.validated_data)
            else:
                self._data = self.get_initial()
        return self._data


class Serializer(BaseSerializer, metaclass=SerializerMetaclass):
    @property
    def data(self):
        ret = super().data
        return ReturnDict(ret, serializer=self)

    def to_representation(self, instance):
        ret = OrderedDict()
        # 1.获取所有的字段：先前加载的 InfoSerializer._declared_fields + Meta中指定的fields字段一一创建对象
        fields = self._readable_fields
        # 2. 循环所有的字段对象(都是序列化器字段对象)
        for field in fields:
            # 去字段对象中依据字段的信息获取数据
            attribute = field.get_attribute(instance)
            # 将获取到的数据通过序列化器类的to_representation方法转换为适合 JSON 序列化的格式加入ret中，如IntegerField会对数据进行int()转换等等
            ret[field.field_name] = field.to_representation(attribute)
        return ret


    @property
    def _readable_fields(self):
        for field in self.fields.values():
            yield field


class Field:
    def __init__(self, ...source):
        self.source = source

    def get_attribute(self, instance):
        # 此处的source_attrs是source经过bind处理后的
        # get_attribute就去instance中取出对应的值，如列表["depart","title"]，会循环去取值，返回instance.depart.title
        return get_attribute(instance, self.source_attrs)

    

# 根据sourse和instance取得对应的值
def get_attribute(instance, attrs):
    for attr in attrs:
        try:
            if isinstance(instance, Mapping):
                instance = instance[attr]
            else:
                instance = getattr(instance, attr)
        except ObjectDoesNotExist:
            return None
        if is_simple_callable(instance):
            try:
                instance = instance()
            except (AttributeError, KeyError) as exc:
                # If we raised an Attribute or KeyError here it'd get treated
                # as an omitted field in `Field.get_attribute()`. Instead we
                # raise a ValueError to ensure the exception is not masked.
                raise ValueError(
                    'Exception raised in callable attribute "{}"; original exception was: {}'.format(attr, exc))

    return instance


class IntegerField(Field):
    def __init__(self, **kwargs):
        self.max_value = 111
        super().__init__(**kwargs)

    def to_representation(self, value):
        return int(value)


class InfoSerializer(serializers.ModelSerializer):
    id = serializers.IntegerField()  # {max_value:111, _creation_counter:0,source="id"}source未定义默认为字段名
    title = serializers.CharField()  # {allow_blank:False, _creation_counter:1}
    order = serializers.IntegerField  # {max_value:111, _creation_counter:2}

    class Meta:
        model = models.Depart
        fields = "__all__"

```

##### 11.2.5.2 ListSerializer类

如果已经了解了UserSerializer类的序列化过程，那么ListSerializer类的序列化只是在他的基础上多了一个循环

ser.data --> ListSerializer中的data -->  BaseSerializer中的data --> self._data = self.to_representation(self.instance)

但是此时的to_representation优先找的是ListSerializer类中自己定义的的

我们先看一下ListSerializer类是如何实例化的

```python
@classmethod
    def many_init(cls, *args, **kwargs):
        child_serializer = cls(*args, **kwargs)  # 实例化UserSerializer对象
        list_kwargs = {
            'child': child_serializer,
        }
        meta = getattr(cls, 'Meta', None)
        list_serializer_class = getattr(meta, 'list_serializer_class', ListSerializer)  # Meta中可以定义，默认为ListSerializer
        return list_serializer_class(*args, **list_kwargs)  # 实例化ListSerializer()，并将每一个UserSerializer对象传入
```

可以看到，ListSerializer类中是一个个UserSerializer对象

因此ListSerializer类在to_representation方法时循环每一个UserSerializer对象并调用它的to_representation方法，显然，就是一遍又一遍的重复我们上一小节分析的UserSerializer类序列化的过程，此处便不再叙述

```python
	    
class ListSerializer(BaseSerializer):
    def to_representation(self, data):
        iterable = data.all() if isinstance(data, models.Manager) else data

        return [
            self.child.to_representation(item) for item in iterable
        ]
```



### 11.3 数据校验

路由 -> 视图 -> request.data -> 校验(序列化器的类) -> 操作数据



接口设计的小规范：

```python
获取数据：
	api/v1/user/		->GET请求		->获取数据列表	models.UserInfo.objects.all()
    api/v1/user/2/		->GET请求		 ->获取单个数据	models.UserInfo.objects.filter(id=2).first()
    
    api/v1/user			->POST请求 	 ->新增数据
    api/v1/user/3/		 ->PUT请求	  ->更新数据
```

下面我们就先看看序列化器的数据校验功能



#### 11.3.1 基本校验

序列化器中定义好字段，当请求到达时，视图函数获取到请求数据，将数据传递到序列化器中进行校验，利用if语句，如果校验通过，返回数据，不通过，返回错误信息

还有第二种写法，在校验时传入`raise_exception=True`参数，校验通过则程序自动向下执行，不通过则抛出异常，由APIView类的dispatch方法接收异常处理

建议采用第一种方式，因为可以自己控制异常的处理，如更改错误信息等，而方式二会由DRF处理，不可控

```python
class DepartSerializer(serializers.Serializer):
    username = serializers.CharField(required=True)
    password = serializers.CharField(required=True)


class DepartView(APIView):
    def post(self, request, *args, **kwargs):
        # 1.获取原始数据
        # print(request.data)
        # 2.校验(写法一)
        ser = DepartSerializer(data=request.data)
        if ser.is_valid():
            print(ser.validated_data)
        else:
            print(ser.errors)

        # 校验(写法二)
        # ser = DepartSerializer(data=request.data)
        # ser.is_valid(raise_exception=True)
        # print(ser.validated_data)

        return Response("...")
```



#### 11.3.2 内置和正则校验

上一小节我们学习了数据校验的基本格式，但对传递来的数据的限制仅仅是非空，当需求增加时，我们需要对字段有更严格的控制，就需要用到更多序列化器内置的要求以及使用正则自定义格式校验

如下，title字段定义了最大长度和最小长度，order定义了最大值和最小值，level字段则是给出了choices，只可选择1或2(输入整形还是字符串并不影响)。这几个校验是序列化器内置的

除此之外我们还可以子定义正则校验，代码中email1和email2使用的正则表达式相同，是DRF给我们封装好的，而email3就是我们通过自己写正则表达式创建的规则，并且可以定制错误信息

```python
from django.core.validators import RegexValidator, EmailValidator


class DepartSerializer(serializers.Serializer):
    title = serializers.CharField(required=True, max_length=20, min_length=6)
    order = serializers.IntegerField(required=False, max_value=100, min_value=10)
    level = serializers.ChoiceField(choices=[("1", "高级"), (2, "中级")])

    # email1 = serializers.EmailField()
    # email2 = serializers.CharField(validators=[EmailValidator(message="邮箱格式错误")])

    email3 = serializers.CharField(validators=[RegexValidator(r"\d+", message="格式错误")])

```



#### 11.3.3 钩子校验

和Django中的Form和ModelForm类似，DRF的序列化器的数据校验也提供了每个字段的钩子方法和一个全局的钩子方法

```python
from rest_framework import exceptions


email = serializers.CharField(validators=[RegexValidator(r"\d+", message="格式错误")])

    def validate_email(self, value):
        print(value)
        if len(value) > 6:
            raise exceptions.ValidationError("字段钩子校验失败")
        return value

    def validate(self, attrs):
        print("validate=", attrs)
        # api_settings.NON_FIELD_ERRORS_KEY
        # raise exceptions.ValidationError("全局钩子校验失败")
        return attrs
```

每个字段的钩子方法以`validate_字段名`命令，而全局的钩子方法直接是`validate`方法，他会在各个字段的钩子方法校验完成后再对整体进行校验，因此传入的attr是所有字段和值组成的有序字典，而单个字段钩子方法的value是单个字段的值

![image-20241224232824750](post\python\DRF\image-20241224232824750.png)

总的钩子方法的错误信息也与字段的钩子不同

![image-20241224233043880](post\python\DRF\image-20241224233043880.png)

它的错误信息的键默认是`non_field_errors`，可以通过`api_settings.NON_FIELD_ERRORS_KEY`进行配置，如：

![image-20241224233734229](post\python\DRF\image-20241224233734229.png)

即可实现自定义错误信息的键



#### 11.3.4 Model校验

上面我们实现数据校验一直是使用Serializer类，所有的字段都需要我们自己编写，如果用到的字段大多都是将数据库中的字段，那么就可以采用ModelSerializer来自动生成字段

```python
class DepartModelSerializer(serializers.ModelSerializer):
    email3 = serializers.CharField(validators=[RegexValidator(r"\d+", message="格式错误")])

    class Meta:
        model = models.Depart
        fields = ['title', 'order', 'count', 'email3']
        extra_kwargs = {
            'title': {"max_length": 5, "min_length": 1},
            'order': {"min_value": 5},
            'count': {"validators": [RegexValidator(r"\d+", message="格式错误")]}
        }
```

只需要在`class Meta`的`fiedls`填入需要生成的字段即可，若还需要自定义字段就像Serializer类一样自定义，然后加入到fields列表中即可

采用这种方式不需要我们自己编写字段，那么校验规则怎么添加呢？

只需要在`class Meta`中`extra_kwargs`字典中编写即可，示例如上



#### 11.3.5 保存数据之普通字段

如果采用的是Serializer类的序列化器，那么在保存数据时需要手动的进行ORM操作保存数据，但如果使用了

ModelSerializer类的序列化器，就可以使用`.save`实现便捷操作，他会一键保存到数据库对应的表中

但是问题来了?如果用户传入的字段数大于或小于数据表对应的字段数，我们能不能手动调整？答案是肯定的

**字段过多**，如下述例子，email3是多余字段，不需要存入数据库，我们可以在进行`.save`之前将其`pop`掉，用一个变量接收，可用于后续判断。该操作可用于注册时的重复输入密码步骤

```python
class DepartModelSerializer(serializers.ModelSerializer):
    email3 = serializers.CharField(validators=[RegexValidator(r"\d+", message="格式错误")])

    class Meta:
        model = models.Depart
        fields = ['title', 'order', 'count', 'email3']
        extra_kwargs = {
            'title': {"max_length": 5, "min_length": 1},
            'order': {"min_value": 5},
            'count': {"validators": [RegexValidator(r"\d+", message="格式错误")]}
        }


class DepartView(APIView):
    def post(self, request, *args, **kwargs):
        ser = DepartModelSerializer(data=request.data)
        if ser.is_valid():
            print("视图", ser.validated_data)
            email3 = ser.validated_data.pop('email3')
            ser.save()

        else:
            print(ser.errors)

        return Response("...")
```

**字段过少**，比如创建用户时数据库中要添加操作人id，这显然不是用户输入的，所以我们就需要手动添加数据

下列代码中数据库有count字段，但前端用户并未输入，因此便可以在`.save`时将count字段作为参数传入

```python
class DepartModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.Depart
        fields = ['title', 'order']
        extra_kwargs = {
            'title': {"max_length": 5, "min_length": 1},
            'order': {"min_value": 5},
        }


class DepartView(APIView):
    def post(self, request, *args, **kwargs):
        ser = DepartModelSerializer(data=request.data)
        if ser.is_valid():
            print("视图", ser.validated_data)
            ser.save(count=11)

        else:
            print(ser.errors)

        return Response("...")
```



#### 11.3.6 保存数据之FK和M2M字段

上一小节我们介绍了普通字段的保存，但是还有两种比较特殊的字段，如果是ForeignKey字段和ManytoMany字段，该怎么进行保存

我们先看FK

![image-20241225104844380](post\python\DRF\image-20241225104844380.png)

如图，depart是一个FK，关联到职位表，用户在输入时实际上输入的是职位的id，如果id不存在就会报错。如果需求升级，就算该职位存在，还要做进一步校验，就需要我们的钩子方法，在钩子中，value值其实已经是用户传入的id对应的depart对象了，我们就可以对对象进行操作、校验

然后我们来看M2M的情况

![image-20241225110343348](post\python\DRF\image-20241225110343348.png)

tags是一个M2M，关联userinfo和tags表，用户传入一个列表，1和2对应tags表的id，表示当前创建用户与id=1和id=2的标签都有关联，于是在userinfo_tags表中生成两条记录

此时，钩子方法中的value是一个列表，每一个元素都是传入的id对应的tags对象

```python
class UsModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.UserInfo
        fields = ['name', 'age', 'gender', 'depart', 'tags']


class USView(APIView):
    def post(self, request, *args, **kwargs):
        ser = UsModelSerializer(data=request.data)
        if ser.is_valid():
            print("视图", ser.validated_data)
            ser.save()

        else:
            print(ser.errors)

        return Response("...")
```

在FK的那个示例中，字段名为depart，但我们传入的实际上是depart_id，那么如果直接在fields中写depart_id而不是depart会发生什么呢?

ModelSerializer在处理数据库字段时其实并没有depart_id这个字段，他只有一个FK字段叫depart，所以他会报错，但是我们可以通过自定义的方式就可以实现

```python
class UsModelSerializer(serializers.ModelSerializer):
    depart_id = serializers.IntegerField()

    class Meta:
        model = models.UserInfo
        fields = ['name', 'age', 'gender', 'depart_id', 'tags']

```

在M2M字段中，用户输入的是存放tags对象的id列表，而到达钩子方法是存放tags对象的列表，那么我们能不能自定义，让他传入钩子时就是用户输入的存放tags对象的id列表，然后可以进一步做操作，最后返回的还是存放tags对象的列表，使保存数据的功能不受影响

```python
class UsModelSerializer(serializers.ModelSerializer):
    tags = serializers.ListField()

    class Meta:
        model = models.UserInfo
        fields = ['name', 'age', 'gender', 'depart', 'tags']

    def validated_tags(self, value):
        print(value)
        queryset = models.Tag.objects.filter(id__in=value)
        return queryset


class USView(APIView):
    def post(self, request, *args, **kwargs):
        ser = UsModelSerializer(data=request.data)
        if ser.is_valid():
            print("视图", ser.validated_data)
            ser.save()

        else:
            print(ser.errors)

        return Response("...")
```



#### 11.3.7 数据校验总结

1. 自定义 Serializer + 字段
2. 自定义 Serializer + 字段（内置加正则）
3. 自定义 Serializer + 字段（内置加正则）+ 字段钩子 + 全局钩子
4. 自定义 ModelSerializer + 自定义字段 + extra_kwargs + save保存数据（字段多：pop；字段少：参数传入）
5. 自定义 ModelSerializer + FK      ->     自动获取关联数据 depart    ->     自定义depart_id字段
6. 自定义 ModelSerializer + M2M   ->     获取关联数据                      ->      ListField或DictField  +  钩子



#### 11.3.8 校验和序列化二合一

序列化与数据校验结合

创建用户 : {"user":"xxx","password":"xxx"}

- 数据校验
- 连接数据库，保存数据，并返回数据库对象
  - 为什么不直接返回创建用户时传递的 {"user":"xxx","password":"xxx"}
  - 因为数据库有些字段时自动生成的，只有返回数据库对象才能保证数据的完整
- 将数据库对象序列化再返回



##### 11.3.8.1 两个序列化器分别实现

我们在数据校验开头部分就提出过这种需求，当数据校验通过存入数据库，还需将创建的数据库对象经过序列化返回回去，现在我们就来看看这个实现

```python
class DpModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.Depart
        fields = "__all__"


class Dp2ModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.Depart
        fields = ["id", "title", "count"]


class DpView(APIView):
    def post(self, request, *args, **kwargs):
        ser = DpModelSerializer(data=request.data)
        if ser.is_valid():
            instance = ser.save()
            print(instance)
            xx = Dp2ModelSerializer(instance=instance)
            return Response(xx.data)

        else:
            return Response(ser.errors)
```

上述代码在数据校验时使用DpModelSerializer，校验通过保存到数据库，`ser.save()`会返回存入数据库的对象，用instance接收，再

传递给完成序列化功能的Dp2ModelSerializer，实现需求

##### 11.3.8.2 一个序列化器实现

这样分开写两个序列化器感觉有点繁杂，可以使用一个吗？答案是可以的，但是返回序列化时返回给用户的也是数据库中的所有字段，因为`fields = "__all__"`

可以只使用一个序列化器，实现数据校验保存数据时校验保存所有字段，但是序列化返回时只返回三个字段吗？这时候就要用到字段的两个特殊参数`read_only`和`write_only`

```python
class DpModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.Depart
        fields = ["id", "title", "order", "count"]
        extra_kwargs = {
            "id": {"read_only": True},
            "count": {"write_only": True},
        }
```

上述代码实现在输入时只需要输入title、order、count三个字段，在返回给用户时只返回id、title、order三个字段。完成共用一个序列化器实现数据校验保存和数据序列化字段的灵活自定义。可以应用在用户注册时，在注册时需要用户输入密码，但是返回数据展示时不会将密码返回

上面我们已经实现了控制哪些字段返回，但是返回的格式还需要进一步操作。例如对于一个choices的gender字段，返回时是返回数据中真实存储的1或2吗，显然不是，而应该是对应的男和女。同样的还有FK字段depart，我们应该单单返回一个depart_id吗？显然也不是，而是应该将其depart_title或其他信息返回，这应该怎么实现呢？

![image-20241225204254510](post\python\DRF\image-20241225204254510.png)

- choices字段

方式一：自定义一个gender_info字段获取男或女的文字信息，然后将gender_info和gender字段分别设置read_only和write_only

```python
class UusModelSerializer(serializers.ModelSerializer):
    gender_info = serializers.CharField(source="get_gender_display", read_only=True)

    class Meta:
        model = models.UserInfo
        fields = ["id", "name", "age", "gender", "depart", "gender_info"]
        extra_kwargs = {
            "id": {"read_only": True},
            "gender": {"write_only": True}
        }
```

方式二：自定义一个方法字段（自带read_only），返回一个字典，包含多个信息

```python
class UusModelSerializer(serializers.ModelSerializer):
    v1 = serializers.SerializerMethodField()

    class Meta:
        model = models.UserInfo
        fields = ["id", "name", "age", "gender", "depart", "v1"]
        extra_kwargs = {
            "id": {"read_only": True},
            "gender": {"write_only": True}
        }

    def get_v1(self, obj):
        return {"id": obj.gender, "text": obj.get_gender_display()}
```

![image-20241225205712042](post\python\DRF\image-20241225205712042.png)

- FK字段

方式一：自定义字段加read_only，指定source关联外键数据

```python
class UusModelSerializer(serializers.ModelSerializer):
    v1 = serializers.CharField(source="depart.title", read_only=True)

    class Meta:
        model = models.UserInfo
        fields = ["id", "name", "age", "gender", "depart", "v1"]
        extra_kwargs = {
            "id": {"read_only": True},
            "depart": {"write_only": True}
        }
```

方式二：利用序列化器的嵌套

```python
class P1ModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.Depart
        fields = "__all__"
        

class UusModelSerializer(serializers.ModelSerializer):
    v1 = P1ModelSerializer(read_only=True, source="depart")

    class Meta:
        model = models.UserInfo
        fields = ["id", "name", "age", "gender", "depart", "v1"]
        extra_kwargs = {
            "id": {"read_only": True},
            "depart": {"write_only": True}
        }
```

![image-20241225210517195](post\python\DRF\image-20241225210517195.png)

如果是二合一的形式，视图函数中可以进一步缩写

```python
class UusView(APIView):
    def post(self, request, *args, **kwargs):
        ser = UusModelSerializer(data=request.data)
        if ser.is_valid():
            ser.save()
            return Response(ser.data)

        else:
            return Response(ser.errors)
```

不需要重复指定数据校验和序列化数据的序列化器，只需如上编写即可实现同样的功能

#### 11.3.9 拓展

现在有一个需求，要求编写一个序列化类，实现创建用户

提供：{"name":"xsss","age":"11","gender":1}

返回：{"id"=1,"name":"xsss","gender":"男"}

model类如下

```python
class NbUserInfo(models.Model):
    name = models.CharField(verbose_name="姓名", max_length=32)
    age = models.IntegerField(verbose_name="年龄")
    gender = models.SmallIntegerField(verbose_name="性别", choices=((1, "男"), (2, "女")))
```

我们上面学习的对choices的处理，都是要新定义一个字段来展示男/女，但需求中不管是提供的字段还是返回的字段都叫gender，显然不是我们原先学习的方法可以实现的

![image-20241226161015680](post\python\DRF\image-20241226161015680.png)

重点就是要将返回的gender设置为男/女

如果我们自定义一个名为gender的SerializerMethodField字段，然后在里面去调用get_gender_display，输出是符合要求了，但是SerializerMethodField字段默认是read_only，会导致传过来的gender失效，即用户无法输入了

如果定义成其他类型的字段，没有read_only，用户可以输入了，但是get_gender的钩子又不执行了，导致输出还是1/2



可以发现，上面两种实现都只实现了一半功能，那没有可能将他们组合起来呢？

这就需要我们了解SerializerMethodField字段到底是如何实现钩子的？

```python
# 序列化
ser = NbModelSerializer(instance=对象)
ser.data

# 数据校验+序列化
ser = NbModelSerializer(data=request.data)
if ser.is_valid():
    ser.save()
    ser.data

# 关键就是ser.data
```

真正的序列化动作发生在ser.data触发时

SerializerMethodField字段与其他字段的不同在于：

- 类变量维护了一个method_name默认等于None
- 在实例化前执行bind方法method_name赋值为'get_{field_name}'
- 最后调用to_representation方法处理值时会找到父类的get_{field_name}，返回钩子方法的返回值

```python
class BaseSerializer(Field):
    @property
    def data(self):
        self._data = self.to_representation(self.instance)
        return self._data


class Serializer(BaseSerializer, metaclass=SerializerMetaclass):
    @property
    def data(self):
        ret = super().data
        return ReturnDict(ret, serializer=self)

    @property
    def _readable_fields(self):
        for field in self.fields.values():
            if not field.write_only:
                yield field

    def to_representation(self, instance):
        ret = OrderedDict()
        # [CharField字段对象，SerializerMethodField字段对象(比其他普通字段多维护了一个method_name)...]  有bind方法会先执行过
        fields = self._readable_fields  # 找到所有字段，筛选出可读的（可序列化的） ==> 没有写write_only

        for field in fields:
            attribute = field.get_attribute(instance)
            # CharField.get_attribute(instance) ==> instance.字段.字段 (通过循环source列表)
            # SerializerMethodField.get_attribute(instance) ==> instance.xx ==> None (SerializerMethodField的source默认为*)
            ret[field.field_name] = field.to_representation(attribute)
            # CharField.to_representation 就是用str()包裹一下
            # SerializerMethodField.to_representation 根据method_name去父类拿钩子方法

        return ret


class SerializerMethodField(Field):
    def __init__(self, method_name=None, **kwargs):
        self.method_name = method_name
        kwargs['source'] = '*'
        kwargs['read_only'] = True
        super().__init__(**kwargs)

    def bind(self, field_name, parent):
        # The method name defaults to `get_{field_name}`.
        if self.method_name is None:
            self.method_name = 'get_{field_name}'.format(
                field_name=field_name)  # method_name默认为None，到此处处理为"get_gender"

        super().bind(field_name, parent)

    def to_representation(self, value):
        method = getattr(self.parent, self.method_name)
        return method(value)


class NbModelSerializer(serializers.ModelSerializer):
    gender = seriallizer.SerializerMethodField()

    class Meta:
        model = models.NbUserInfo
        fields = ["id", "name", "age", "gender"]
        extra_kwargs = {
            "id": {"read_only": True},
        }

    def get_gender(self, obj):
        return obj.get_gender_display()
```

明白了它是如何实现钩子方法之后，我们就可以将原先的两种方式进行结合，关键就在于对下面两条语句的拓展，因为他们决定了字段的实际值

- `attribute = field.get_attribute(instance)`
- `ret[field.field_name] = field.to_representation(attribute)`

##### 11.3.9.1 方法一

我们通过自定义一个NbCharField字段，模仿SerializerMethodField维护一个method_name，并且找时机让他根据反射找到钩子并执行即可

下列实现在`__init__`和`__bind__`中维护一个钩子方法，名为`xget_字段名`，然后在执行get_attribute时让他执行钩子，将1/2变为男/女，因为我们继承的是IntegerField，它的to_representation方法应该是用int()包裹attribute，而我们的attribute不是数字字符串，因此to_representation也需要重写，否则强加int()会报错，至此就完成了功能实现

```python
class NbCharField(serializers.IntegerField):
    def __init__(self, method_name=None, **kwargs):
        self.method_name = method_name
        super().__init__(**kwargs)

    def bind(self, field_name, parent):
        if self.method_name is None:
            self.method_name = 'xget_{field_name}'.format(field_name=field_name)

        super().bind(field_name, parent)

    def get_attribute(self, instance):
        method = getattr(self.parent, self.method_name)
        return method(instance)

    def to_representation(self, value):
        return str(value)


class NbModelSerializer(serializers.ModelSerializer):
    gender = NbCharField()

    class Meta:
        model = models.NbUserInfo
        fields = ["id", "name", "age", "gender"]
        extra_kwargs = {
            "id": {"read_only": True},
        }

    def xget_gender(self, obj):
        return obj.get_gender_display()


class NbView(APIView):
    def post(self, request, *args, **kwargs):
        ser = NbModelSerializer(data=request.data)
        if ser.is_valid():
            ser.save()
            return Response(ser.data)

        else:
            return Response(ser.errors)
```

可以看到，通过自定义一个字段的方式还是比较繁琐的



##### 11.3.9.2 方法二

方法一的本质是改变字段的`get_attribute`和`to_representation`方法来改变获取字段的值，而这两个方法都定义Serializer类的`to_representation`方法中，那么我们何不直接重写一个`to_representation`方法，使得当字段存在名为nb_字段名的钩子方法时，走我们自己的获取数据流程，而没有名为`nb_字段名`的钩子方法时，就按原本的逻辑走



```python
class NbModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.NbUserInfo
        fields = ["id", "name", "age", "gender"]
        extra_kwargs = {
            "id": {"read_only": True},
        }

    def to_representation(self, instance):
        ret = OrderedDict()
        fields = self._readable_fields

        for field in fields:
            if hasattr(self, 'nb_%s' % field.field_name):
                value = getattr(self, "nb_%s" % field.field_name)(instance)
                ret[field.field_name] = value
            else:
                try:
                    attribute = field.get_attribute(instance)
                except SkipField:
                    continue

                check_for_none = attribute.pk if isinstance(attribute, PKOnlyObject) else attribute
                if check_for_none is None:
                    ret[field.field_name] = None
                else:
                    ret[field.field_name] = field.to_representation(attribute)

        return ret

    def nb_gender(self, obj):
        return obj.get_gender_display()


class NbView(APIView):
    def post(self, request, *args, **kwargs):
        ser = NbModelSerializer(data=request.data)
        if ser.is_valid():
            ser.save()
            return Response(ser.data)

        else:
            return Response(ser.errors)
```

如果我们还有其他需要序列化器需要实现这个功能，是否需要在每一个序列化器中自定义一遍`to_representation`？

我们可以将自定义的方法打包到一个类中，在需要实现该功能的序列化器类中继承即可

`ext/hook.py`

```python
from collections import OrderedDict
from rest_framework.fields import SkipField
from rest_framework.relations import PKOnlyObject


class NbHookSerializer(object):
    def to_representation(self, instance):
        ret = OrderedDict()
        fields = self._readable_fields

        for field in fields:
            if hasattr(self, 'nb_%s' % field.field_name):
                value = getattr(self, "nb_%s" % field.field_name)(instance)
                ret[field.field_name] = value
            else:
                try:
                    attribute = field.get_attribute(instance)
                except SkipField:
                    continue

                check_for_none = attribute.pk if isinstance(attribute, PKOnlyObject) else attribute
                if check_for_none is None:
                    ret[field.field_name] = None
                else:
                    ret[field.field_name] = field.to_representation(attribute)

        return ret

```

`views.py`

```python
from ext.hook import NbHookSerializer


class NbModelSerializer(NbHookSerializer, serializers.ModelSerializer):
    class Meta:
        model = models.NbUserInfo
        fields = ["id", "name", "age", "gender"]
        extra_kwargs = {
            "id": {"read_only": True},
        }

    def nb_gender(self, obj):
        return obj.get_gender_display()


class NbView(APIView):
    def post(self, request, *args, **kwargs):
        ser = NbModelSerializer(data=request.data)
        if ser.is_valid():
            ser.save()
            return Response(ser.data)

        else:
            return Response(ser.errors)
```



### 11.4 博客系统案例

开发一个博客系统，包含：博客列表、详细、登录、注册、评论、点赞、发布博客

#### 11.4.1 表结构

项目的创建在此不赘述，翻看先前文章即可

先创建表结构，models.py内容如下

```python
from django.db import models


class UserInfo(models.Model):
    username = models.CharField(verbose_name="用户名", max_length=32, db_index=True)
    password = models.CharField(verbose_name="密码", max_length=64)
    token = models.CharField(verbose_name="TOKEN", max_length=64, null=True, blank=True,db_index=True)


class Blog(models.Model):
    category_choices = ((1, "云计算"), (2, "Python全栈"), (3, "Go开发"))
    category = models.IntegerField(verbose_name="分类", choices=category_choices)

    image = models.CharField(verbose_name="封面", max_length=255)
    title = models.CharField(verbose_name="标题", max_length=32)
    summary = models.CharField(verbose_name="简介", max_length=256)
    text = models.TextField(verbose_name="博文")
    ctime = models.DateTimeField(verbose_name="创建时间", auto_now_add=True)
    creator = models.ForeignKey(verbose_name="创建者", to="UserInfo", on_delete=models.CASCADE)

    comment_count = models.PositiveIntegerField(verbose_name="评论数", default=0)
    favor_count = models.PositiveIntegerField(verbose_name="赞数", default=0)


class Favor(models.Model):
    """ 赞 """
    blog = models.ForeignKey(verbose_name="博客", to="Blog", on_delete=models.CASCADE)
    user = models.ForeignKey(verbose_name="用户", to="UserInfo", on_delete=models.CASCADE)
    create_datetime = models.DateTimeField(verbose_name="创建时间", auto_now_add=True)

    class Meta:
        constraints = [
            models.UniqueConstraint(fields=['blog', 'user'], name='uni_favor_blog_user')
        ]


class Comment(models.Model):
    """ 评论表 """
    blog = models.ForeignKey(verbose_name="博客", to="Blog", on_delete=models.CASCADE)
    user = models.ForeignKey(verbose_name="用户", to="UserInfo", on_delete=models.CASCADE)

    content = models.CharField(verbose_name="内容", max_length=150)
    create_datetime = models.DateTimeField(verbose_name="创建时间", auto_now_add=True)
```

做一个数据库迁移

```
makemigrations
migrate
```

添加测试数据(可以采用离线脚本的形式，或者创建一个内容如下的接口，通过访问来创建)

```python
from django.shortcuts import render, HttpResponse
from api import models


# Create your views here.
def db(request):
    v1 = models.UserInfo.objects.create(username='qwj', password='123')
    v2 = models.UserInfo.objects.create(username='xzq', password='123')

    models.Blog.objects.create(
        category=1,
        image="xxx/xxxx.pbg",
        title='郑经理',
        summary='...',
        text='说大话电话卡文化的哇',
        creator=v1,
    )
    models.Blog.objects.create(
        category=2,
        image="xxx/xxxx.pbg",
        title='证据',
        summary='...',
        text='大家啊我到家啊五角大楼',
        creator=v2,
    )
    return HttpResponse("成功")

```

数据如下

![image-20250109120757629](post\python\DRF\image-20250109120757629.png)

#### 11.4.2 功能实现

##### 11.4.2.1 博客列表

路由

```python
path('api/blog/', views.BlogView.as_view()),
```

视图类

```python
class BlogView(APIView):
    def get(self, request, *args, **kwargs):
        """获取博客列表"""

        # 1.读取数据库中的博客信息
        queryset = models.Blog.objects.all().order_by('-id')

        # 2.序列化
        ser = BlogSerializer(queryset, many=True)

        # 3.返回
        context = {'code': 1000, 'data': ser.data}
        return Response(context)
```

序列化器

- 写法一：自定义字段

```python
class BlogSerializer(serializers.ModelSerializer):
    category = serializers.CharField(source="get_category_display")
    ctime = serializers.DateTimeField(format="%Y-%m-%d")
    creator_name = serializers.CharField(source="creator.username")

    class Meta:
        model = models.Blog
        fields = ["category", "image", "title", "summary", "ctime", "comment_count", "favor_count", "creator", "creator_name"]
```

- 写法二：自定义方法

```python
class BlogSerializer(serializers.ModelSerializer):
    category = serializers.CharField(source="get_category_display")
    ctime = serializers.DateTimeField(format="%Y-%m-%d")
    creator = serializers.SerializerMethodField()

    class Meta:
        model = models.Blog
        fields = ["category", "image", "title", "summary", "ctime", "comment_count", "favor_count", "creator"]

    def get_creator(self, obj):
        return {"id": obj.creator_id, "name": obj.creator.username}
```

- 写法三：嵌套

```python
class BlogUserSerializers(serializers.ModelSerializer):
    class Meta:
        model = models.UserInfo
        fields = ["id", "username"]


class BlogSerializer(serializers.ModelSerializer):
    category = serializers.CharField(source="get_category_display")
    ctime = serializers.DateTimeField(format="%Y-%m-%d")
    creator = BlogUserSerializers()

    class Meta:
        model = models.Blog
        fields = ["category", "image", "title", "summary", "ctime", "comment_count", "favor_count", "creator"]

```



##### 11.4.2.2 博客详细与评论

路由

```python
path('api/blog/<int:pk>/', views.BlogDetailView.as_view()),
path('api/comment/<int:blog_id>/', views.CommentView.as_view()),
# path('api/comment/', views.CommentView.as_view()),
```

> 获取博客评论的接口可以写成`api/comment?blog_id=1`，但是不太美观，建议使用`api/comment/1`然后在视图中接收

博客详细

```python
class BlogDetailSerializers(serializers.ModelSerializer):
    category = serializers.CharField(source="get_category_display")
    ctime = serializers.DateTimeField(format="%Y-%m-%d")
    creator = BlogUserSerializers()

    class Meta:
        model = models.Blog
        fields = "__all__"


class BlogDetailView(APIView):
    def get(self, request, *args, **kwargs):
        """获取博客详细"""

        # 1.获取ID
        pk = kwargs.get('pk')

        # 2.根据ID获取对象
        instance = models.Blog.objects.filter(id=pk).first()
        if not instance:
            return Response({'code': 1000, 'data': "不存在"})

        # 3.序列化
        ser = BlogDetailSerializers(instance, many=False)

        # 4.返回
        context = {'code': 1000, 'data': ser.data}
        return Response(context)
```

评论

```python
class CommentSerializers(serializers.ModelSerializer):
    user = serializers.CharField(source="user.username")

    class Meta:
        model = models.Comment
        fields = ["id", "content", "user"]


class CommentView(APIView):
    def get(self, request, blog_id):
        """评论列表"""

        # 1.根据ID获取评论对象
        queryset = models.Comment.objects.filter(blog_id=blog_id)

        # 2.序列化
        ser = CommentSerializers(queryset, many=True)

        # 3.返回
        context = {'code': 1000, 'data': ser.data}
        return Response(context)
```

测试数据还是通过访问/db/来添加:

```python
# 在def db(request)中添加如下操作
models.Comment.objects.create(content="yi", blog_id=1, user_id=1)
models.Comment.objects.create(content="er", blog_id=1, user_id=2)
```

结果如下

![image-20250109164431239](post\python\DRF\image-20250109164431239.png)

基本功能已经实现了，但是现在只是评论的展示，后续如果用户要添加评论时，user字段是需要用户传入id值的，而展示则是展示用户名，所以此处需要我们进行一个拓展，让该字段在校验时传入的是id，在序列化时是返回用户名

用到先前提到的自定义`NbHookSerializer`类实现

在`etc/hook.py`中定义

```python
from collections import OrderedDict
from rest_framework.fields import SkipField
from rest_framework.relations import PKOnlyObject


class NbHookSerializer(object):
    def to_representation(self, instance):
        ret = OrderedDict()
        fields = self._readable_fields

        for field in fields:
            if hasattr(self, 'nb_%s' % field.field_name):
                value = getattr(self, "nb_%s" % field.field_name)(instance)
                ret[field.field_name] = value
            else:
                try:
                    attribute = field.get_attribute(instance)
                except SkipField:
                    continue

                check_for_none = attribute.pk if isinstance(attribute, PKOnlyObject) else attribute
                if check_for_none is None:
                    ret[field.field_name] = None
                else:
                    ret[field.field_name] = field.to_representation(attribute)

        return ret

```

对`CommentSerializers`类做如下修改即可

```python
from ext.hook import NbHookSerializer


class CommentSerializers(NbHookSerializer, serializers.ModelSerializer):
    class Meta:
        model = models.Comment
        fields = ["id", "content", "user"]

    def nb_user(self, obj):
        return obj.user.username
```



##### 11.4.2.3 注册

路由

```python
path('api/register/', views.RegisterView.as_view()),
```

视图+序列化

> 字段的钩子方法执行顺序按fields中定义的顺序来
>
> 通过定义read_only和write_only区别数据校验和序列化字段的异同
>
> self.initial_data可以拿到传入的所有数据
>
> 数据保存时字段过多的处理方法：pop

```python
class RegisterSerializers(serializers.ModelSerializer):
    confirm_password = serializers.CharField(write_only=True)

    class Meta:
        model = models.UserInfo
        fields = ["id", "username", "password", "confirm_password"]
        extra_kwargs = {
            "id": {"read_only": True},
            "password": {"write_only": True},
        }

    def validate_password(self, value):
        print("密码：", value)
        return value

    def validate_confirm_password(self, value):
        print("重复密码：", value)
        password = self.initial_data.get('password')
        if password != value:
            raise ValidationError("密码不一致")
        return value


class RegisterView(APIView):
    def post(self, request):
        """注册"""
        # 1.提交数据
        # {"username":"qiuwj","password":"123","confirm_password":"123"}

        # 2.数据校验+保存
        ser = RegisterSerializers(data=request.data)
        if ser.is_valid():
            confirm_password = ser.validated_data.pop('confirm_password')
            ser.save()
            return Response({'code': 1000, 'data': ser.data})
        else:
            return Response({'code': 1001, 'error': "注册失败", 'detail': ser.errors})

```

##### 11.4.2.4 登录

路由

```python
path('api/login/', views.LoginView.as_view()),
```

视图加序列化

```python
class LoginSerializers(serializers.ModelSerializer):
    class Meta:
        model = models.UserInfo
        fields = ["username", "password"]


class LoginView(APIView):
    def post(self, request):
        """登录"""
        # 1.提交数据
        # {"username":"qiuwj","password":"123"}

        # 2.数据校验+保存
        ser = LoginSerializers(data=request.data)
        if not ser.is_valid():
            return Response({'code': 1001, 'error': "校验失败", 'detail': ser.errors})

        instance = models.UserInfo.objects.filter(**ser.validated_data).first()
        if not instance:
            return Response({'code': 1002, 'error': "用户名或密码错误"})
        token = str(uuid.uuid4())
        instance.token = token
        instance.save()
        return Response({'code': 1000, 'token': token})

```



##### 11.4.2.5 发布评论

发表评论可以采用单独写一个视图类的序列化类来实现，但更规范的写法是整合到上面的博客评论中，当GET请求时，是展示评论，当POST请求时，是发布评论

因为发布评论必须先登录，因此要用到认证类，但是展示评论不需要登录，而这两个功能是写在一个视图类中的，所以我们可以采用匿名用户，当未登录时不让他抛出错误而是返回None，最后如果request.user不为None则说明登陆了，如果是None说明是未登录，不让发表。

![image-20250112160054842](post\python\DRF\image-20250112160054842.png)

在请求参数中携带token作身份标识，在请求体中传递评论内容

认证类

```python
from rest_framework.authentication import BaseAuthentication
from api import models


class BlogAuthentication(BaseAuthentication):
    def authenticate(self, request):
        token = request.query_params.get('token')
        if not token:
            return

        instance = models.UserInfo.objects.filter(token=token).first()
        if not instance:
            return

        return instance, token

    def authenticate_header(self, request):
        return 'API'

```



视图类+序列化类

```python
class CommentSerializers(NbHookSerializer, serializers.ModelSerializer):
    class Meta:
        model = models.Comment
        fields = ["id", "content", "user"]
        extra_kwargs = {
            "id": {"read_only": True},
            "user": {"read_only": True},
        }

    def nb_user(self, obj):
        return obj.user.username


class CommentView(APIView):
    authentication_classes = [BlogAuthentication, ]

    def get(self, request, blog_id):
        """评论列表"""

        # 1.根据ID获取评论对象
        queryset = models.Comment.objects.filter(blog_id=blog_id)

        # 2.序列化
        ser = CommentSerializers(queryset, many=True)

        # 3.返回
        context = {'code': 1000, 'data': ser.data}
        return Response(context)

    def post(self, request, blog_id):
        """发布评论"""
        if not request.user:
            return Response({'code': 3000, 'data': "认证失败"})

        blog_object = models.Blog.objects.filter(id=blog_id).first()
        if not blog_object:
            return Response({'code': 2000, 'data': "博客不存在"})

        ser = CommentSerializers(data=request.data)
        if not ser.is_valid():
            return Response({'code': 1002, 'data': "内容审核不通过"})

        ser.save(blog=blog_object, user=request.user)
        return Response({'code': 1000, 'data': ser.data})

```



##### 11.4.2.6 点赞

路由

```python
path('api/favor/', views.FavorView.as_view()),
```



因为点赞功能必须登录，因此新增`NoAuthentication`认证类，当`BlogAuthentication`类为校验通过时，就抛出错误

认证类

``` python
from rest_framework.authentication import BaseAuthentication
from api import models
from rest_framework import exceptions


class BlogAuthentication(BaseAuthentication):
    def authenticate(self, request):
        token = request.query_params.get('token')
        if not token:
            return

        instance = models.UserInfo.objects.filter(token=token).first()
        if not instance:
            return

        return instance, token

    def authenticate_header(self, request):
        return 'API'


class NoAuthentication(BaseAuthentication):
    def authenticate(self, request):
        raise exceptions.AuthenticationFailed({"code": 2000, "error": "认证失败"})

    def authenticate_header(self, request):
        return 'API'

```

序列化+视图函数

```python
class FavorSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.Favor
        fields = ["id", "blog"]


class FavorView(APIView):
    authentication_classes = [BlogAuthentication, NoAuthentication, ]

    def post(self, request):
        ser = FavorSerializer(data=request.data)
        if not ser.is_valid():
            return Response({'code': 1002, 'error': "校验失败", 'detail': ser.errors})

        # 1.已存在 不赞
        exists = models.Favor.objects.filter(user=request.user, **ser.validated_data).exists()
        if exists:
            return Response({'code': 1005, 'error': "已赞过，不允许重复点赞"})

        # 2.不存在
        ser.save(user=request.user)
        return Response({'code': 1000, 'data': ser.data})
```



##### 12.4.2.7 新建博客

和查看评论与发布评论相同，博客列表与新建博客也放到一个视图类中

因为新建博客必须登录，可以采取匿名用户+对`request.user`判断的形式来做权限校验

视图类

> 关键在于对get和post请求时不同字段的隐藏和显示处理

```python
class BlogSerializer(NbHookSerializer, serializers.ModelSerializer):
    ctime = serializers.DateTimeField(format="%Y-%m-%d", read_only=True)
    creator = BlogUserSerializers(read_only=True)

    class Meta:
        model = models.Blog
        fields = ["id", "category", "image", "title", "summary", "text", "ctime", "comment_count", "favor_count",
                  "creator"]
        extra_kwargs = {
            "comment_count": {"read_only": True},
            "favor_count": {"read_only": True},
            "text": {"write_only": True},
        }

    def nb_category(self, obj):
        return obj.get_category_display()


class BlogView(APIView):
    authentication_classes = (BlogAuthentication,)

    def get(self, request, *args, **kwargs):
        """获取博客列表"""

        # 1.读取数据库中的博客信息
        queryset = models.Blog.objects.all().order_by('-id')

        # 2.序列化
        ser = BlogSerializer(queryset, many=True)

        # 3.返回
        context = {'code': 1000, 'data': ser.data}
        return Response(context)

    def post(self, request, *args, **kwargs):
        if not request.user:
            return Response({'code': 3000, 'data': "认证失败"})
        ser = BlogSerializer(data=request.data)
        if not ser.is_valid():
            return Response({'code': 1002, 'error': "校验失败", 'detail': ser.errors})
        ser.save(creator=request.user)
        return Response({'code': 1000, "data": ser.data})
```

实现效果

![image-20250120233733760](post\python\DRF\image-20250120233733760.png)

#### 12.4.3 总结

主要拓展了权限类的或关系以及关于序列化器传入与输出的字段名相同但内容不同的拓展

但存在几个问题

- 点赞评论后博客对应点赞数和评论数未变化 -->  f操作自加一
- 博客列表和评论列表的分页功能

案例完整项目命名为blog1.zip



## 12. 分页

在django中原生的分页组件不够好用，于是我们自定义了一个组件，而在drf中，他为我们提供了三个还不错的分页组件，下面我们来介绍一下比较常用的两个

`PageNumberPagination`，适用于显示页面、上一下、下一页

> /accounts/?page=4
> /accounts/?page=6
> /accounts/?page=7
> /accounts/?page=7&page_size=1000000000
> /accounts/?page=7&page_size=2
> /accounts/?page=7&page_size=4

后端给前端返回的数据参考，由前端自己划分


	res = {
		"data":[xxx,xxxx,xxxx,xxx,xxxx],
		"total":1000,
		"persize":20
	}

-------------------------------------------------

`LimitOffsetPagination`，滚动翻页

> 从第二条开始,取十条
>
> /accounts/?offset=2&limit=10
> /accounts/?offset=10&limit=10
>
> 还可以添加条件,如id大于10的中从第0个开始,取十条
>
> /accounts/?lastid=10&offset=0&limit=10
> /accounts/?lastid=20&offset=0&limit=10



### 12.1 PageNumberPagination

#### 12.1.1 初步使用

![image-20250121154003508](post\python\DRF\image-20250121154003508.png)

在url中传入不同的page,可以返回对应页的数据

> 一共有三篇博客,page_size=2,因此第一页两条数据,第二页一条数据

![image-20250121154415485](post\python\DRF\image-20250121154415485.png)

将该分页组件应用到前面的案例中,新的BlogView类实现如下

```python
class BlogView(APIView):
    authentication_classes = (BlogAuthentication,)

    def get(self, request, *args, **kwargs):
        """获取博客列表"""

        # 1.读取数据库中的博客信息
        queryset = models.Blog.objects.all().order_by('-id')

        # 分页
        from rest_framework.pagination import PageNumberPagination
        pager = PageNumberPagination()
        result = pager.paginate_queryset(queryset, request, self)

        # 2.序列化
        ser = BlogSerializer(result, many=True)

        # 3.返回
        context = {'code': 1000, 'data': ser.data}
        return Response(context)
```

写了三行代码,改了一个值就实现了分页功能



#### 12.1.2 自定义PageNumberPagination类

通过继承PageNumberPagination类,自定义一些字段的值实现定制

![image-20250121155536954](post\python\DRF\image-20250121155536954.png)

```python
class MyPageNumberPagination(PageNumberPagination):
    page_size_query_param = 'size'
    page_query_param = 'p'
    max_page_size = 2
    page_size = 1


class BlogView(APIView):
    authentication_classes = (BlogAuthentication,)

    def get(self, request, *args, **kwargs):
        """获取博客列表"""

        # 1.读取数据库中的博客信息
        queryset = models.Blog.objects.all().order_by('-id')

        # 分页
        pager = MyPageNumberPagination()
        result = pager.paginate_queryset(queryset, request, self)

        # 2.序列化
        ser = BlogSerializer(result, many=True)

        # 3.返回
        context = {'code': 1000, 'data': ser.data}
        return Response(context)
```



#### 12.1.3 分页的返回值处理

将序列化后的数据丢入`get_paginated_response`函数,封装返回值

```python
class BlogView(APIView):
    authentication_classes = (BlogAuthentication,)

    def get(self, request, *args, **kwargs):
        """获取博客列表"""

        # 1.读取数据库中的博客信息
        queryset = models.Blog.objects.all().order_by('id')

        # 2.处理得到分页后的queryset
        from rest_framework.pagination import PageNumberPagination
        pager = PageNumberPagination()
        result = pager.paginate_queryset(queryset, request, self)

        # 3.序列化
        ser = BlogSerializer(result, many=True)

        # 4.获取分页返回结果
        response = pager.get_paginated_response(ser.data)
        return response
```

`get_paginated_response`函数实现逻辑

```python
class PageNumberPagination(BasePagination):
    def get_paginated_response(self, data):
        return Response(OrderedDict([
            ('count', self.page.paginator.count),
            ('next', self.get_next_link()),
            ('previous', self.get_previous_link()),
            ('results', data)
        ]))
```

就是封装了一个有序字典并放到response中,如果不满意它的内容可以自定义`PageNumberPagination`类重写`get_paginated_response`方法,手动封装

#### 12.1.4 终极使用

例子

```python
class BlogView(APIView):
    authentication_classes = (BlogAuthentication,)

    def get(self, request, *args, **kwargs):
        """获取博客列表"""

        # 1.读取数据库中的博客信息
        queryset = models.Blog.objects.all().order_by('id')

        # 2.处理得到分页后的queryset
        from rest_framework.pagination import PageNumberPagination
        pager = PageNumberPagination()
        result = pager.paginate_queryset(queryset, request, self)

        # 3.序列化
        ser = BlogSerializer(result, many=True)

        # 4.获取分页返回结果
        response = pager.get_paginated_response(ser.data)
        return response
```



### 12.2 LimitOffsetPagination

#### 12.2.1 基本使用及拓展

`LimitOffsetPagination`与`PageNumberPagination`的使用相同，唯一的区别就是实例化的类不同，还有url中传入的参数不同

也可以通过继承，自定义一个类，修改其中的参数，例如下列

```
default_limit = api_settings.PAGE_SIZE
limit_query_param = 'limit'
offset_query_param = 'offset'
max_limit = None
```

使用方法如下

```python
class BlogView(APIView):
    authentication_classes = (BlogAuthentication,)

    def get(self, request, *args, **kwargs):
        """获取博客列表"""

        # 1.读取数据库中的博客信息
        queryset = models.Blog.objects.all().order_by('id')

        # 2.处理得到分页后的queryset
        from rest_framework.pagination import LimitOffsetPagination
        pager = LimitOffsetPagination()
        result = pager.paginate_queryset(queryset, request, self)

        # 3.序列化
        ser = BlogSerializer(result, many=True)

        # 4.获取分页返回结果
        response = pager.get_paginated_response(ser.data)
        return response
```



#### 12.2.2 实际使用

在项目中，很少将`LimitOffsetPagination`用于普通的分页需求中，而是主要用来实现滚动翻页，比如页面默认显示10条，当滚到底部时才加载后10条，以此类推

为什么不使用`PageNumberPagination`呢？我们举个例子来理解

比如一组倒序排列的数10,9,8...1

第一次offset=0&limit=2取到10，9

第二次offset=2&limit=2取到8，7

正常来讲前端记录取出的个数然后作为offset不会出错，但如果插入了新的数据到最前端

第三次offset=0&limit=2取到7，6就会出现重复

解决办法就是前端记录取出的最后一个数据的id作为max_id，这样就只会从未取出的数据中查找，offset=0&limit=2无需更改

要拿最新数据只需要限制条件为id大于最上面的一条数据的id即可，即可实现向上滚动翻页和向下滚动翻页

> /accounts/?offset=0&limit=2&lastid=0
> 	13	武沛齐
> 	12	武沛齐     max_id=12
>
> /accounts/?offset=0&limit=2&lastid=12
> 	11	武沛齐
> 	10	武沛齐      max_id=10
>
> /accounts/?offset=0&limit=2&lastid=10
> 	9	武沛齐
> 	8	武沛齐

案例

```python
class BlogView(APIView):
    authentication_classes = (BlogAuthentication,)

    def get(self, request, *args, **kwargs):
        """获取博客列表"""

        # 1.读取数据库中的博客信息
        queryset = models.Blog.objects.all().order_by('-id')

        # ?max_id=1
        # ?min_id=13
        max_id = request.query_params.get('max_id', None)
        if max_id:
            queryset = queryset.filter(id__lt=max_id)

        # 2.处理得到分页后的queryset
        from rest_framework.pagination import LimitOffsetPagination
        pager = LimitOffsetPagination()
        result = pager.paginate_queryset(queryset, request, self)

        # 3.序列化
        ser = BlogSerializer(result, many=True)

        # 4.获取分页返回结果
        response = pager.get_paginated_response(ser.data)
        return response
```



