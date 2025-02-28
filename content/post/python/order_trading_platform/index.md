---
title: "项目：订单交易平台"
slug: "Order_trading_platform"
description: "基于Django开发的前后端不分离的订单交易平台"
date: "2025-02-27T18:57:50+08:00"
lastmod: "2025-02-27T18:57:50+08:00"
image: 
categories: ["python"]
tags: ["Django","python"]
---
当时跟着学的时候还没养成做笔记的习惯，因此该文是事后复习时写的，内容有些凌乱，见谅

<!--more-->

核心的功能模块

+ 认证模块，用户名密码登录或收集短信登录（60s有效）
+ 角色管理，不同角色有不同权限和展示不同菜单
+ 客户管理，对客户分级，客户的增删改查操作
+ 交易中心
    - 管理员给客户充值扣费
    - 客户可以下单撤单
    - 生成交易记录
    - 对订单进行多维度搜索
+ wordker，执行订单并更新订单状态

## 单点知识
### 发送短信
阿里云短信服务来进行发送短信

+ 注册账号
+ 开通服务 + 缴费
+ API服务、SDK服务
    - API接口

```python
import requests

# 处理签名和加密
res = requests.get("......",params={"key":"xxx",'token':'...'})
```

    - SDK服务

```python
pip install aliyun-python-sdk-core
pip install aliyun-python-sdk-dysmsapi
```

```python
import xxxx

xxxxx.send(...)
```



示例

仅作参考，具体推荐依照官方文档进行配置

```python
from aliyunsdkcore.client import AcsClient
from aliyunsdkcore.request import CommonRequest
import json


def sendSms(phone, message):
    # 替换为你的AccessKey信息
    try:
        ACCESS_KEY_ID = ''
        ACCESS_KEY_SECRET = ''
        SIGN_NAME = ''  # 签名名称
        TEMPLATE_CODE = ''  # 模板CODE
        PHONE_NUMBER = phone  # 绑定的测试手机号

        acs_client = AcsClient(ACCESS_KEY_ID, ACCESS_KEY_SECRET, TEMPLATE_CODE)

        request = CommonRequest()
        request.set_accept_format('json')
        request.set_domain('dysmsapi.aliyuncs.com')
        request.set_method('POST')
        request.set_version('2017-05-25')
        request.set_action_name('SendSms')

        # 设置短信模板参数
        request.add_query_param('PhoneNumbers', PHONE_NUMBER)
        request.add_query_param('SignName', SIGN_NAME)
        request.add_query_param('TemplateCode', TEMPLATE_CODE)
        request.add_query_param('TemplateParam', json.dumps({"code": message}))  # 模板中的变量替换为实际的值

        # 发送短信请求并获取返回结果
        response = acs_client.do_action_with_exception(request)
        response_str = response.decode('utf-8')
        response_dict = json.loads(response_str)
        if response_dict['Code'] == "OK":
            return True
        return False
    except Exception as e:
        pass
```



### 权限和菜单管理
#### 菜单
+ 页面写死

```python
<html>
    {% if 角色 "管理员"%}
    	<a href="/xxx/x">用户管理</a>
    	<a href="/xxx/x">级别管理</a>
    	<a href="/xxx/x">级别管理</a>
    	...
    {% else %}
    	<a href="/xxx/x">xxx管理</a>
    	<a href="/xxx/x">级别管理</a>
    {% endif %}
</html>
```

+ 菜单放在配置文件

```python
# settings.py

ADMIN = [
    {"title":"用户管理", "url":"...." },
    {"title":"用户管理", "url":"...." },
    {"title":"用户管理", "url":"...." },
    {"title":"用户管理", "url":"...." },
]

USER = [
    {"title":"用户管理", "url":"...." },
    {"title":"用户管理", "url":"...." },
    {"title":"用户管理", "url":"...." },
    {"title":"用户管理", "url":"...." },
]
```

```python
<html>
    {% if 角色 "管理员"%}
    	{% for item in ADMIN%}
		    <a href="{{item.url}}">{{item.title}}</a>
	    {%emdfor%}
    {% else %}
    	{% for item in USER%}
		    <a href="{{item.url}}">{{item.title}}</a>
	    {%emdfor%}
    {% endif %}
</html>
```

+ 将菜单 + 角色写入数据库

![](/post/Python/order_trading_platform/1732339379585-36fe528c-6de1-4063-8dd3-410112a3d40c.png)

```python
# 在页面展示数据库
# 1.查询特定角色关联的所有的菜单

# 2.在页面上进行展示
```

如果选择配置文件的方式：

+ 菜单配置

```python
ADMIN = [
    {
        "title":"用户管理", 
        "children":[
            {"title":"级别列表","url":"....", "name":"level_list",}
            {"title":"级别列表","url":"...."}
            {"title":"级别列表","url":"...."}
        ]
    },
    {
        "title":"订单管理", 
        "children":[
            {"title":"订单列表","url":"...."}
            {"title":"订单列表","url":"...."}
            {"title":"订单列表","url":"...."}
        ]
    },
]
```

+ 菜单选中和展开问题

```python
1.获取当前用户请求的 URL pricepolicy/list/ 或 url对应的name

2. pricepolicy/list/ 配置 ADMIN中的URL   ->默认选中
```

+ 路径导导航问题

```python
1.获取当前用户请求的 URL   pricepolicy/list/ 或 url对应的name

2.获取上级，展示导航信息

3.设置菜单与下级关系
```

拓展：菜单多级关系

![](/post/Python/order_trading_platform/1732339989906-a9856733-c818-43dd-8bac-d93df45a45fa.png)

#### 权限
权限的判定，要考虑：正常的点击、非法的输入

+ 文件settings.py的方式

```python
admin_permisions = {
    "level_list":{...},
    "level_edit":{..., 'parent':'level_list'},
    "level_add":{... 'parent':'level_list'},
    "level_delete":{..'parent':'level_list'.},
    
    "user_list":{...},
    "user_edit":{...},
    "user_add":{...},
    "user_delete":{...},
}

user_permisions = {
    ...
}
```

```python
admin访问某个URL + 路由信息（name，namespace），获取当前URL /level/dedit/4/ -> 是否存在URL
```

```python
在中间件中根据URL中的name进行权限的校验
```

+ 数据库方式

![](/post/Python/order_trading_platform/1732349583777-e301bcc2-78a6-4951-bd36-e46f5e864674.png)



#### 实现的前置小知识
+ 中间件

```python
process_request，基于他实现用户是否已登录，继续；未登录则返回登录界面。
	- return None，继续向后访问
	- return 对象，直接返回。
	
process_view，权限校验
	- return None，继续向后访问
	- return 对象，直接返回。
	- 在他的request对象中有 resolver_match  ，包含当前请求的视图路由信息  .name->sms_login
		admin = ['sms_login',"xxx"]
	
process_response
```

+ 面向对象

```python
mapping = {"1": "ADMIN", "2": "CUSTOMER"}
request.session['user_info'] = {'role': mapping[role], 'name': user_object.username, 'id': user_object.id}
```



### ajax请求
+ form表单

```python
<form method='get' action='xxx' >
    <input />
    <input type='submit' />
</form>
```

+ ajax

Ajax（Asynchronous JavaScript and XML）是一种在无需重新加载整个页面的情况下，与服务器交换数据并更新部分网页的技术，本质上就是利用浏览器上XMLRequest

**基本步骤：**

+ 创建XMLRequest对象
+ 初始化请求：指定请求类型以及请求的URL
+ 发送请求
+ 处理响应
+ 更新页面

```python
<html>
    <head>
        
    </head>
    <body>
        <script>
        	 xhr = new XMLHttpRequest();
             xhr.onreadystatechange = function(){
                 if(xhr.readyState == 4){
                    // 已经接收到全部响应数据，执行以下操作
                    var data = xhr.responseText;
                    console.log(data);
                }
             };
             xhr.open('POST', "/test/", true );
             xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded;');
             xhr.send('n1=1;n2=2;');
        </script>
    </body>
</html>
```

利用jQuery类库，内部封装，使用简单了

```python
<html>
	...
    <body>
        
        <script src='jquery.js'></script>
        <script>
        	$.ajax({
                url:"....",
                type:"post",
                data:{n1:123,n2:456},
                success:function(res){
                    console.log(res);
                }
            })
        </script>
    </body>
</html>
```

大致应用场景：

+ 提交数据，页面可以刷新 -> form表单
+ 提交数据，页面不想刷新 -> ajax形式提交

### csrftoken
#### form表单
```python
<form ...>
    {% csrf_token %}  
	<!-- 表单字段 -->
    <input type="submit" value="Submit">
</form>
```

#### ajax方式
浏览器打开网址时，django在cookie中也给我们返回了一段值。

```python
$.ajax({
	url:"...",
	type:"get"
	data:{user:"wupeiqi",pwd:"xxxx"}，
	header:{
		"X-HTTP...":"cookie中写的那段值"
	},
	success:function(arg){
		
	}
})
```

#### 向后端发送请求
+ form - GET

```python
<form method='GET'>
    
</form>
```

+ form - POST

```python
<form method='POST'>
    {% csrf_token %}
    ...
</form>
```

+ ajax - GET

```python
$.ajax({
    url: "/xxx/xx",
    type: "GET",
    data: {mobile: "1888888"},
    success: function (res) {
        console.log(res);
    }
})
```

+ ajax - POST

```python
$.ajax({
    url: "/xxx/xx",
    type: "POST",
    data: {mobile: "1888888"},
    headers:{
        "X-CSRFTOKEN":"...."
    },
    success: function (res) {
        console.log(res);
    }
})
```

+ ajax - PUT

```python
$.ajax({
    url: "/xxx/xx",
    type: "PUT",
    data: {mobile: "1888888"},
    headers:{
        "X-CSRFTOKEN":"...."
    },
    success: function (res) {
        console.log(res);
    }
})
```

+ ajax - DELETE

```python
$.ajax({
    url: "/xxx/xx",
    type: "DELETE",
    data: {mobile: "1888888"},
    headers:{
        "X-CSRFTOKEN":"...."
    },
    success: function (res) {
        console.log(res);
    }
})
```

+ ajax - serialize

> 携带所有参数，包括csrftoken
>

```python
$.ajax({
    url:"/sms/login/",
    type:"POST",
    data:$("#f1").serialize()
})

# 好处，不需要加csrf header
```



#### csrf认证
+ 请求体中

```python
- 传统的form
- jQuery    $("#smsForm").serialize()  + Ajax
```

+ 请求体中

```python
$.ajaxSetup({
	beforeSend...
})

$.ajax({
	url:"...",
	type:"GET",
	data:{},
	dataType:"JSON",
	headers:{
		...
	},
	success:function(res){
	}
})
```

#### django的自定义请求头
```python
- 自动化添加 HTTP_ 前缀
- 自动化添加 HTTP_ 前缀，前端的-，转换成后端的_
```



### form组件
#### 自定义表单类
Django提供了两种自定义表单的方式：继承`Form`类和`ModelForm`类。前者你需要自定义表单中的字段，后者可以根据Django模型自动生成表单，如下：

```python
# app/forms.py
# 自定义表单字段
from django import forms
from .models import Contact

class ContactForm1(forms.Form):
    name = forms.CharField(label="Your Name", max_length=255)
    email = forms.EmailField(label="Email address")

# 根据模型创建
class ContactForm2(forms.ModelForm):
    
    class Meta:
        model = Contact
        fields = ('name', 'email',)
```

> `label用来给字段添加一个描述`
>

#### 自定义错误字段信息
```python
from django import forms

class LoginForm(forms.Form):  
    username = forms.CharField(
        required=True,
        max_length=20,
        min_length=6,
        error_messages={
            'required': '用户名不能为空',
            'max_length': '用户名长度不得超过20个字符',
            'min_length': '用户名长度不得少于6个字符',
        }
    )
    password = forms.CharField(
        required=True,
        max_length=20,
        min_length=6,
        error_messages={
            'required': '密码不能为空',
            'max_length': '密码长度不得超过20个字符',
            'min_length': '密码长度不得少于6个字符',
        }
    )
```

对于基继承`ModelForm`类的表单, 我们可以在`Meta`选项下widget中来自定义错误信息

```python
from django.forms import ModelForm, Textarea
from myapp.models import Author

class AuthorForm(ModelForm):
    class Meta:
        model = Author
        fields = ('name', 'title', 'birth_date')
        widgets = {
            'name': Textarea(attrs={'cols': 80, 'rows': 20}),  # 关键是这一行
        }
        labels = {
            'name': 'Author',
        }
        help_texts = {
            'name': 'Some useful help text.',
        }
        error_messages = {
            'name': {
                'max_length': "This writer's name is too long.",
            },
        }
```

#### 自定义表单输入`widget`
Django表单的每个字段你都可以选择你喜欢的输入`widget`，比如多选，复选框。你还可以定义每个widget的css属性

```python
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField(
        max_length=255,
        widget=forms.Textarea(
            attrs={'class': 'custom'},
        ),
    )
```

#### 表单的实例化与初始化
定义好一个表单类后，你就可以对其进行实例化或初始化。下面方法可以实例化一个空表单，但里面没有任何数据，可以通过 `{{ form }}`在模板中渲染

```python
form = ContactForm() # 空表单
```

有时我们需要对表单设置一些初始数据，我们可以通过`initial`方法或设置`default_data`，如下所示

```python
# initial方法初始化
form = ContactForm(
    initial={
        'name': 'First and Last Name',
    },)

# default_data默认值
default_data = {'name': 'John', 'email': 'someone@hotmail.com', }
form = ContactForm(default_data)
```

用户提交的数据可以通过以下方法与表单结合，生成与数据结合过的表单(Bound forms)。Django只能对Bound forms进行验证

```python
form = ContactForm(data=request.POST, files=request.FILES)
```

其编辑修改类应用场景中，我们还要给表单提供现有对象实例的数据，而不是渲染一张空表单，这时我们可这么做。该方法仅适用于由模型创建的`ModelForm`，而不适用于自定义的表单

```python
contact = Contact.objects.get(id=1)
form =  ContactForm(instance = contact, data=request.POST)
```

#### 表单的使用
#### 继承Form类
+ 编写字段

自定义编写

+ 生成HTML标签 + 携带数据

```python
- 保留原来提交的数据，不再担心form表单提交时页面刷新。
- 显示默认值，做编辑页面显示默认值。
```

+ 数据校验，对用户提交的数据格式校验

```python
form = LoginForm(data=request.POST)
if form.is_valid():
    print(form.cleaned_data)
else:
    print(form.errors)
```



#### 继承ModelForm类
+ 编写字段

选择字段

```python
class LevelModelForm(forms.ModelForm):
    class Meta:
        model = models.Level  # 指定引用的表名
        fields = ['title', 'percent']  # 引用的字段
        fields = "__all__"  # 引用全部字段
		exclude = ['active']  # 排除某一字段
```

添加自定义字段

```python
class LevelModelForm(BootStrapForm, forms.ModelForm):
    xxx = forms.CharField(label='xxx')

    class Meta:
        model = models.Level
        # fields = "__all__"
        # exclude = ['active']
        fields = ['title', 'xxx', 'percent', ]
```

重写字段

```python
class LevelModelForm(BootStrapForm, forms.ModelForm):
    title = forms.ChoiceField(label='xxx', choices=((1, "xxx"), (2, "xxxxxx")))

    class Meta:
        model = models.Level
        # fields = "__all__"
        # exclude = ['active']
        fields = ['title', 'percent', ]
```

+ 生成HTML标签 + 插件 + 参数的配置

```python
class LevelModelForm(BootStrapForm, forms.ModelForm):
    class Meta:
        model = models.Level
        fields = ['title', 'percent', ]
        widgets = {
            'name': forms.PasswordInput(render_value=True)
        }
```

+ 表单验证

```python
class LevelModelForm(BootStrapForm, forms.ModelForm):
    title = forms.CharField(validators=[])
    class Meta:
        model = models.Level
        fields = ['title', 'percent', ]

    def clean_percent(self):
        value = self.cleaned_data['percent']
        return value
```

+ 保存更新

```python
form = LevelModelForm(data=request.POST)
form.save()
```

```python
form = LevelModelForm(data=request.POST,install=对象)
form.save()
```

```python
# 输入字段少于数据库，手动添加
# 输入字段多余数据库b
form = LevelModelForm(data=request.POST) # 或有 ,install=对象

form.instance.percent = 10
form.save()
```



#### form的校验流程
+ 每个字段的内部：required + validators + min_length + max_length
+ 每个字段的钩子方法

```python
def clean_username(self):
    user = self.cleaned_data['username']
    # 校验规则
    # 校验失败
    if len(user) < 3:
        from django.core.exceptions import ValidationError
        raise ValidationError("用户名格式错误")
	return user

print(form.cleaned_data)
```

+ clean

```python
def clean(self):
    # 对所有值进行校验
    from django.core.exceptions import ValidationError
    # 1.不返回值，默认 self.cleaned_data
    # 2.返回值，self.cleaned_data=返回的值
    # 3.报错，ValidationError ->  self.add_error(None, e)
```

+ _post_clean

```python
def _post_clean(self):
    pass
```



+ form目的：生成标签 + 校验
+ 生成标签
    - 循环 + 单独某个字段
    - label页面显示文本信息
    - 自定义错误位置，position
+ 校验
    - 定义
        * 每个字段校验
            + 字段上定义校验规则 正则、空、长度
            + clean_方法名

```python
校验成功：
	self.cleaned_data["字段"] = 值
校验失败
	self.errors["字段"] = 错误
```

        * 所有字段校验
            + clean

```python
try:
    cleaned_data = self.clean()
except ValidationError as e:
    self.add_error(None, e)
else:
    if cleaned_data is not None:
        self.cleaned_data = cleaned_data
        
self.errors["__all__"] = 错误
```

            + _post_clean
    - 校验源码
        * form.is_valid
        * form.errors
        * form.fuull_clean()
    - 业务校验

```python
form = 对象

if not form.is_valid():
    print(form.errors)
    form.errors.username.0
    form.errors.__call__.0   ->  模板语言中 form.errors.__ 不支持
    form.non_field_errors()  <==> form.errors.__call__
    在页面上去适合的位置显示错误信息
else:
    print(form.cleaned_data)
    ...
```

![](/post/Python/order_trading_platform/1732377343476-dfd6b6b1-1b53-4861-b14a-40c18d8dfacc.png)

    - 拓展

```python
如果想要让某个错误信息，展示在特定的字段旁，就可以使用：
form.add_error("password", "用户名或密码错误")
```

![](/post/Python/order_trading_platform/1732377389035-fddedf5e-b105-4d35-80bd-b17ab91f489a.png)



#### form样式设置
思考：无论在使用Form还是ModelForm时，想要让页面好看，就需要将每个字段的插件中给他设置form-control样式

##### 循环写入
```python
class LevelForm(forms.Form):
    title = forms.CharField(
        label="标题",
        required=True,
        # widget=forms.TextInput(attrs={"class": "form-control", 'placeholder': "请输入标题"}),
    )
    percent = forms.CharField(
        label="折扣",
        required=True,
        help_text="填入0-100整数表示百分比，例如：90，表示90%"
    )

    def __init__(self,*args,**kwargs):
        super().__init__(*args,**kwargs)
        
        # {'title':对象,"percent":对象}
        for name,field in self.fields.items():
            field.widget.attrs['class'] = "form-control"
            field.widget.attrs['placeholder'] = "请输入{}".format(field.label)
```

```python
class LevelModelForm(forms.ModelForm):
    class Meta:
        model = models.Level
        fields = ['title', 'percent']
        
    def __init__(self,*args,**kwargs):
        super().__init__(*args,**kwargs)
        
        # {'title':对象,"percent":对象}
        for name,field in self.fields.items():
            field.widget.attrs['class'] = "form-control"
            field.widget.attrs['placeholder'] = "请输入{}".format(field.label)
```

##### Bootstrap样式优化
##### super关键字
```python
super是找父类中的成员吗？不准确。
super,就是根据类的继承关系（mro），逐步晚上找。
```

```python
class Base:
    def f1(self):
        print("Base")


class Foo(Base):
	pass

class Bar(Foo):
    def f1(self):
        print("Bar")
        super().f1()


obj = Bar()  # [Bar,Foo,Base]
obj.f1()
# Bar Base
```

```python
class Base:
    def f1(self):
        print("Base")

class Foo:
    def f1(self):
        print("Foo")
        super().f1()
        
class Bar(Foo,Base):
    def f1(self):
        print("Bar")
        super().f1()
        
obj = Bar()  # [Bar,Foo,Base]
obj.f1()
# Bar Foo Base
```

```python
class Base:
    def f1(self):
        print("Base")

class Foo:
    def f1(self):
        print("Foo")
        super().f1()
        
class Bar(Foo,Base):
    def f1(self):
        print("Bar")
        super().f1()
        
obj = Foo()
obj.f1()
# Foo
```









```python
from django.shortcuts import render, redirect
from web import models
from django import forms
from django.urls import reverse


class BootStrapForm:
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)

        # {'title':对象,"percent":对象}
        for name, field in self.fields.items():
            field.widget.attrs['class'] = "form-control"
            field.widget.attrs['placeholder'] = "请输入{}".format(field.label)


class LevelForm(BootStrapForm, forms.Form):
    title = forms.CharField(
        label="标题",
        required=True,
    )
    percent = forms.CharField(
        label="折扣",
        required=True,
        help_text="填入0-100整数表示百分比，例如：90，表示90%"
    )


class LevelModelForm(BootStrapForm, forms.ModelForm):
    class Meta:
        model = models.Level
        fields = ['title', 'percent']
```

##### 局部去除bootstrap样式 + 修改字段样式
![](/post/Python/order_trading_platform/1732416059542-81dab9ba-f776-472d-8ab7-f55dfc0324fb.png)

原理：

![](/post/Python/order_trading_platform/1732416076683-02a8e815-2209-4c5d-ace3-ef48620a4ed6.png)





#### form组件基本操作
##### 显示默认值
+ Form

```python
form = LevelForm(initial={'title': "xxx"})
```

+ ModelForm

```python
form = LevelForm(initial={'title': "xxx"})

level_object = models.Level.objects.filter(id=pk).first()
form = LevelModelForm(instance=level_object)
```



### Model
#### 修改查询条件
+ models中的limit_choice_to字段

![](/post/Python/order_trading_platform/1732415713151-2d59c2e8-efcd-451a-a38e-34eca240a6da.png)

+ 自定义choices和queryset

![](/post/Python/order_trading_platform/1732415742966-cf853046-b791-41e2-a730-d50c7f021ef2.png)

传入request以便后续条件复杂化

### 模板
#### 母版




#### inclusion_tag


### 队列
![](/post/Python/order_trading_platform/1732349610537-4e709222-39a3-4489-b719-1cd4e1008d0e.png)

+ rabbitMQ，Linux命令+服务构建+python代码。
+ kafka，Linux命令+服务构建+python代码。
+ redis的列表

![](/post/Python/order_trading_platform/1732349626708-a5c87e69-4f8d-42f0-816e-4751167eb4aa.png)



基于redis实现上述功能的过程和代码示例：

+ 安装redis
+ 启动redis
+ python操作redis

```python
pip install redis
```

```python
import redis

conn = redis.Redis(host='127.0.0.1', port=6379, password='qwe123', encoding='utf-8')

# 短信验证码
conn.set('15131255089', 9999, ex=10)
value = conn.get('15131255089')
print(value)
```

```python
import redis

conn = redis.Redis(host='127.0.0.1', port=6379, password='qwe123', encoding='utf-8')

# 放值
# conn.lpush('my_queue', "root")
# conn.lpush('my_queue', "good")

# 取值
v1 = conn.brpop("my_queue", timeout=5)
print(v1)
```

### worker和线性池
```python
# 1.去redis中获取任务

# 2.再将此订单在数据库中的状态修改为 执行中

# 3.获取任务详细： 视频，10000

# 4.线程池或协程
from concurrent.futures import ThreadPoolExecutor


def task(video_url):
    # 根据视频地址实现刷播放
    # ..
    pass


pool = ThreadPoolExecutor(50)
for i in range(10000):
    pool.submit(task, "视频地址")

pool.shutdown()  # 卡主，等待所有的任务执行完毕。

# 5.更新订单状态，已完成
```



### 分页组件
#### 选项生成
![](/post/Python/order_trading_platform/1732416637414-b8a1fbc2-d88a-49e0-b546-5326800b9acc.png)

#### 页码逻辑和动态页码
![](/post/Python/order_trading_platform/1732416707994-199c0f3d-c70c-4632-9c20-c95ab4bea4ea.png)



#### 分页组件的封装
![](/post/Python/order_trading_platform/1732416742666-835e5124-e6cd-41aa-a4a2-489e5b271587.png)

#### 优化
加入首页、尾页、上一页、下一页

![](/post/Python/order_trading_platform/1732416785524-9beb4d22-b0bb-4af7-80c0-e50edd5b94f9.png)

#### querydict
```python
http://127.0.0.1:8000/customer/list/?filter=wupeiqi&age=19

request.GET  对象    QueryDict类型

1.默认QueryDict不允许被修改 _mutable=False
	request.GET._mutable = True

2.设置值
	request.GET.setlist("name",[123])    # filter=wupeiqi&age=19&name=123
	request.GET.setlist("name",[123,32]) # filter=wupeiqi&age=19&name=123&name=32

3.调用urlencode方法，可以拼接
	paramString = request.GET.urlencode()
	print(paramString) "filter=wupeiqi&age=19&name=123"

------测试-----
import copy

query_dict = copy.deepcopy(request.GET)

query_dict._mutable = True
query_dict.setlist('page',[1])
paramString = query_dict.urlencode() "filter=wupeiqi&age=19&page=1"


query_dict._mutable = True
query_dict.setlist('page',[2])
paramString = query_dict.urlencode() "filter=wupeiqi&age=19&page=2"


request.GET._mutable = True
request.GET.setlist('page',[3])
paramString = request.GET.urlencode() "filter=wupeiqi&age=19&page=3"
```

#### 解决url参数携带问题
![](/post/Python/order_trading_platform/1732416996115-a1389f86-7e9a-44bc-98c3-4d33f67f01b7.png)

#### 最终版






### 搜索组件
#### Q对象
```python
django中有个Q对象

models.XX.objects.filter(id=1)
models.XX.objects.filter(id__gt=1)
models.XX.objects.filter(id__gte=1)
models.XX.objects.filter(name__contains="xx")
models.XX.objects.filter(id=1, age=19)

方式1：
	models.XX.objects.filter( Q(id=10) )	
	models.XX.objects.filter( Q(id=10)&Q(age=19) )	
	models.XX.objects.filter( Q(id=10)|Q(age=19) )	
	models.XX.objects.filter( Q(id__gt=10)|Q(age__lte=19) )	
	models.XX.objects.filter( Q( Q(id__gt=10)|Q(age__lte=19) ) & Q(name=19))	

方式2：
	q1 = Q()
	q1.connector = 'OR'
	q1.children.append(('id', 1))
	q1.children.append(('age', 10))      id=1 or age=10


	q2 = Q()
	q2.connector = 'ADN'
	q2.children.append(('size__gt', 10))
	q2.children.append(('name', 'root'))   size>10 and  name=root


	con = Q()
	con.add(q1, 'AND')
    con.add(q2, 'AND')
    (id=1 or age=10 )  AND (size>10 and  name=root)
```





#### 普通搜索








#### 组合搜索




### 权限按钮
没权限就不要显示

#### 在模板中自定义方法
+ filter

```python
"xxxx"|upper
```

+ simple_tag

不能用if做条件

```python
{% xxxx x1 x2 x3 %}
```

```python
def xxx():
	return ""
```

+ inclusion_tag

```python
def xxx():
	return {'v1':xx,'v2':xx}
```

```python
<h1>{{v1}}</h1>
```



#### 反向生成URL
```python
path('login/', account.login, name="login")
reverse("login")

path('level/edit/<int:pk>/', level.level_edit,name="level_edit")
reverse("level/edit",kwargs={"pk":123})

re_path(r'level/(\d+)/', level.level_edit,name="level")
reverse("level/edit",wargs=(111))
```



#### 实现
权限的粒度控制在按钮，根据是否具有权限来决定是否展示 响应按钮。

```python
{% permission request "user_add" v1=111 v2=111 %}
```

```python
@register.simple_tag
def demo(request, name, *args, **kwargs):
    print(request, name, args, kwargs)
    # 1.读取用户角色，获取权限字典
    
    # 2.判断是否有权限
    
    # 3.无权限，返回空
    
    # 4.有权限，根据name生成URL
    return "...."
```

![](/post/Python/order_trading_platform/1732417614947-651a16d8-3a0e-41dd-af35-d4f3ab9236b9.png)

```python
生成内容分为按钮的生成和表单中td标签的生成和th标点的生成
客户的增删改查与价格策略的差不多，删除操作都是生成一个cid在标签里后续通过js发送至后端，
但级别管理是通过url:level/delete/1来接收pk然后直接删除，没有模态框等操作，因此重写delete_url_permission方法
```

![](/post/Python/order_trading_platform/1732419352138-59ac7616-92c9-4bf0-bd2c-b807c364e1b8.png)



### 跳转链接
在生成URL时，需要读取当前URL中的参数并构造URL。例如：

当前URL

```python
http://127.0.0.1:8000/customer/list/?keyword=xinchen&page=8
```

构造当前页面URL

```python
http://127.0.0.1:8000/customer/edit/1/?_filter=keyword%3Dxinchen%26page%3D8
```

```python
param = request.GET.urlencode() # keyword=xinchen&page=8

new_query_dict = QueryDict(mutable=True)
new_query_dict['_filter'] = param

new_query_dict.urlencode()  _filter=
```

跳转回来时

```python
http://127.0.0.1:8000/customer/list/?keyword=xinchen&page=8
```

```python
def policy_edit(request, pk):
    ...
    base_url = reverse(name, args=args, kwargs=kwargs)
    param_url = request.GET.get('_filter')
    url = "{}?{}".format(base_url,param_url)
    return redirect(url)
```

![](/post/Python/order_trading_platform/1732420375791-3a55f11f-dbde-49a1-8afb-dc818037c834.png)



### 事务
#### 介绍
innodb引擎中支持事务，myisam不支持

例如：A 给 B 转账 100，那就会涉及2个步骤。

+ A 账户 减100
+ B 账户 加 100

这两个步骤必须同时完成才算完成，并且如果第一个完成、第二步失败，还是回滚到初始状态。

事务，就是来解决这种情况的。  大白话：要成功都成功；要失败都失败。

事务的具有四大特性（ACID）：

+ 原子性（Atomicity）

```python
原子性是指事务包含的所有操作不可分割，要么全部成功，要么全部失败回滚。
```

+ 一致性（Consistency）

```python
执行的前后数据的完整性保持一致。
```

+ 隔离性（Isolation）

```python
一个事务执行的过程中,不应该受到其他事务的干扰。
```

+ 持久性（Durability）

```python
事务一旦结束,数据就持久到数据库
```



#### MySQL客户端
+ 设置隔离级别

```python
NSACTION ISOLATION LEVEL [隔离级别];

常见隔离级别：
    READ UNCOMMITTED：允许读取未提交的数据。
    READ COMMITTED：只能读取已提交的数据。
    REPEATABLE READ：确保在同一事务中多次读取同样的记录结果是一致的。
    SERIALIZABLE：最高的隔离级别，完全串行化的事务。
```



+ 开始事务

```python
START TRANSACTION;
# BEGIN;
```



+ 执行数据库操作

```python
在事务中，你可以执行多个数据库操作，如INSERT、UPDATE、DELETE等。这些操作将被包含在当前事务中
```

+ 提交或回滚事务

```python
提交事务
COMMIT;

回滚所有操作
ROLLBACK;
```

#### Python代码
```python
import pymysql

conn = pymysql.connect(host='127.0.0.1', port=3306, user='root', passwd='root123', charset="utf8", db='userdb')
cursor = conn.cursor()

# 开启事务
conn.begin()

try:
    cursor.execute("update users set amount=1 where id=1")
    int('asdf')
    cursor.execute("update tran set amount=2 where id=2")
except Exception as e:
    # 回滚
    print("回滚")
    conn.rollback()
else:
    # 提交
    print("提交")
    conn.commit()

cursor.close()
conn.close()
```

#### Django操作
```python
from django.db import transaction

with transaction.atomic():
	# 数据库操作A
    # 数据库操作B
```



### 锁
#### 介绍
在用MySQL时，不知你是否会疑问：同时有很多做更新、插入、删除动作，MySQL如何保证数据不出错呢？



MySQL中自带了锁的功能，可以帮助我们实现开发过程中遇到的同时处理数据的情况。对于数据库中的锁，从锁的范围来讲有：

+ 表级锁，即A操作表时，其他人对整个表都不能操作，等待A操作完之后，才能继续。
+ 行级锁，即A操作表时，其他人对指定的行数据不能操作，其他行可以操作，等待A操作完之后，才能继续。

```python
MYISAM支持表锁，不支持行锁；
InnoDB引擎支持行锁和表锁。

即：在MYISAM下如果要加锁，无论怎么加都会是表锁。
   在InnoDB引擎支持下如果是基于索引查询的数据则是行级锁，否则就是表锁。
```

所以，一般情况下我们会选择使用innodb引擎，并且在 搜索 时也会使用索引（命中索引）。

在innodb引擎中，update、insert、delete的行为内部都会先申请锁（排它锁），申请到之后才执行相关操作，最后再释放锁。

所以，当多个人同时像数据库执行：insert、update、delete等操作时，内部加锁后会排队逐一执行。

而select则默认不会申请锁。

如果，你想要让select去申请锁，则需要配合 事务 + 特殊语法来实现。



`for update`，排它锁，加锁之后，其他不可以读写

```python
begin; 
	select * from L1 where name="武沛齐" for update;    -- name列不是索引（表锁）
commit;
```

```python
begin; -- 或者 start transaction;
	select * from L1 where id=1 for update;			  -- id列是索引（行锁）
commit;
```

基于Python代码示例

```python
import pymysql
import threading


def task():
    conn = pymysql.connect(host='127.0.0.1', port=3306, user='root', passwd='root123', charset="utf8", db='userdb')
    cursor = conn.cursor(pymysql.cursors.DictCursor)
    # cursor = conn.cursor()
	
    # 开启事务
    conn.begin()

    cursor.execute("select id,age from tran where id=2 for update")
    # fetchall      ( {"id":1,"age":10},{"id":2,"age":10}, )   ((1,10),(2,10))
    # {"id":1,"age":10}   (1,10)
    result = cursor.fetchone()
    current_age = result['age']
    
    if current_age > 0:
        cursor.execute("update tran set age=age-1 where id=2")
    else:
        print("已售罄")

    conn.commit()

    cursor.close()
    conn.close()


def run():
    for i in range(5):
        t = threading.Thread(target=task)
        t.start()


if __name__ == '__main__':
    run()
```



### Message组件
![](/post/Python/order_trading_platform/1732720463327-c9cff61e-1c73-4592-a43f-b2363cdf865d.png)

#### 配置
```python
# MESSAGE_STORAGE = 'django.contrib.messages.storage.fallback.FallbackStorage'
# MESSAGE_STORAGE = 'django.contrib.messages.storage.cookie.CookieStorage'
MESSAGE_STORAGE = 'django.contrib.messages.storage.session.SessionStorage'
```

如图四个位置

![](/post/Python/order_trading_platform/1732720505524-787489d0-acec-4b44-9f2a-0f284c23571d.png)

#### 读取值
```python
from django.contrib.messages.api import get_messages
messages = get_messages(request)
for msg in messages:
    print(msg)
```



```python
<ul>
    {% for message in messages %}
	    <li>{{ message.tags }} {{ message }}</li>
    {% endfor %}
</ul>
```

![](/post/Python/order_trading_platform/1732720567644-3d4b2423-3498-4e03-8101-48bf4dff412d.png)

#### 源码分析
前戏：message是一个对象（包裹）。

```python
v1 = "wupeiqi"
v2 = ["武沛齐",123]

class Message(object):
    def __init__(self, level, message, extra_tags=None):
        self.level = int(level)
        self.message = message
        self.extra_tags = extra_tags
		
obj = Message(10,"哈哈哈哈","123")
```

```python
from django.contrib import messages
messages.add_message(reqeust, messages.ERROR, "删除成功1")
messages.add_message(reqeust, messages.SUCCESS, "删除成功2", extra_tags="哈哈哈")
```

![](/post/Python/order_trading_platform/1732720676596-a3032643-a0f7-4073-a3ff-abe7f170a6eb.png)

![](/post/Python/order_trading_platform/1732720682764-31163ba5-8c3d-485a-a48d-a78521419e80.png)

+ 【设置】中间件process_request加载
+ 【设置】在视图函数中往message中写入值（内存）
+ 【设置】中间件process_response，将内存中新增的数据写入到数据源
+ 【新页面】中间件process_request加载
+ 【新页面】在视图函数或模板中读取message中的信息（老的数据源加载的+新增的）
+ 【设置】中间件process_response

```python
used = True，则只保存新增部分。
added_new = True，老的数据源加载的+新增的都重新保存到数据源。
```

流程分析：

+ 1.1 process_request

![](/post/Python/order_trading_platform/1732720747603-5775175f-9420-481a-90af-d35b154cf25f.png)

![](/post/Python/order_trading_platform/1732720752615-2aa212e3-bc17-4946-a4be-4ba5cd7bb10f.png)

```python
给request._message赋值了一个SessionStorage对象
```

+ 1.2 删除视图

![](/post/Python/order_trading_platform/1732720777652-624b9187-f46d-4c6e-8cf4-a26680c26721.png)

+ 1.3 request_response

![](/post/Python/order_trading_platform/1732720791532-35f3a55a-352d-4d01-900b-5118d007448d.png)

+ 2.1 process_request + 列表视图

![](/post/Python/order_trading_platform/1732720810924-4c5bbd80-c48a-41ef-a072-c04da375676b.png)

+ 2.2 request_response

![](/post/Python/order_trading_platform/1732720828091-ff87ebff-38e7-4373-9f97-01fd9b82ada5.png)



### 组合搜索
具体实现：略



#### 使用
在视图函数中

```python
from django.shortcuts import render
from web import models
from utils.pager import Pagination
from django.db.models import Q
from utils.group import Option, NbSearchGroup


def my_transaction_list(request):
    """我的交易记录"""
    # 第一步：配置和传参
    search_group = NbSearchGroup(
        request,
        models.TransactionRecord,
        Option('charge_type'), # choice
    )

    # 第二步：获取条件
    queryset = models.TransactionRecord.objects.filter(**search_group.get_condition)
    pager = Pagination(request, queryset)

    # 第三步：传入前端页面
    context = {
        "pager": pager,
        "keyword": keyword,
        "search_group_row_list": search_group
    }

    return render(request, "my_transaction_list.html", context)

```

```python
#母版
<link rel="stylesheet" href="{% static 'css/search-group.css' %}">
```

模板

```python
{% block content %}
    {% include 'include/search_group.html' %}
{% endblock %}
```





### 杂


#### 在全局变量中定义自己的专属配置
```python
from django.conf import settings
在settings文件中定义专属配置（名称必须大写）
```

#### 面向对象小知识
##### super关键字
```python
super是找父类中的成员吗？不准确。
super,就是根据类的继承关系（mro），逐步晚上找。
```

```python
class Base:
    def f1(self):
        print("Base")


class Foo(Base):
	pass

class Bar(Foo):
    def f1(self):
        print("Bar")
        super().f1()


obj = Bar()  # [Bar,Foo,Base]
obj.f1()
# Bar Base
```

```python
class Base:
    def f1(self):
        print("Base")

class Foo:
    def f1(self):
        print("Foo")
        super().f1()
        
class Bar(Foo,Base):
    def f1(self):
        print("Bar")
        super().f1()
        
obj = Bar()  # [Bar,Foo,Base]
obj.f1()
# Bar Foo Base
```

```python
class Base:
    def f1(self):
        print("Base")

class Foo:
    def f1(self):
        print("Foo")
        super().f1()
        
class Bar(Foo,Base):
    def f1(self):
        print("Bar")
        super().f1()
        
obj = Foo()
obj.f1()
# Foo
```







## 项目
**初始化基本流程：**

+ 新建项目、虚拟环境、pip、project、app
+ 新建数据库、配置数据库连接、精简化django
+ 创建表结构（注册app、注释admin）



**urls.py路由配置：**

```python
# from django.contrib import admin
from django.urls import path
from web.views import account
from web.views import level
from web.views import customer
from web.views import policy
from web.views import my_order
from web.views import my_transaction

urlpatterns = [
    # path('admin/', admin.site.urls),
    path('login/', account.login, name="login"),
    path('sms/login/', account.sms_login, name="sms_login"),
    path('sms/send/', account.sms_send, name="sms_send"),
    path('logout/', account.logout, name="logout"),

    path('home/', account.home, name="home"),
    path('level/list/', level.level_list, name="level_list"),
    path('level/add/', level.level_add, name="level_add"),
    path('level/edit/<int:pk>/', level.level_edit, name="level_edit"),
    path('level/delete/<int:pk>/', level.level_delete, name="level_delete"),

    path('customer/list/', customer.customer_list, name="customer_list"),
    path('customer/add/', customer.customer_add, name="customer_add"),
    path('customer/edit/<int:pk>/', customer.customer_edit, name="customer_edit"),
    path('customer/reset/<int:pk>/', customer.customer_reset, name="customer_reset"),
    path('customer/delete/', customer.customer_delete, name="customer_delete"),
    path('customer/charge/<int:pk>/', customer.customer_charge, name="customer_charge"),
    path('customer/charge/<int:pk>/add', customer.customer_charge_add, name="customer_charge_add"),

    path('policy/list/', policy.policy_list, name="policy_list"),
    path('policy/add/', policy.policy_add, name="policy_add"),
    path('policy/edit/<int:pk>/', policy.policy_edit, name="policy_edit"),
    path('policy/delete/', policy.policy_delete, name="policy_delete"),

    path('my/order/list/', my_order.my_order_list, name="my_order_list"),
    path('my/order/add/', my_order.my_order_add, name="my_order_add"),
    path('my/order/cancel/<int:pk>/', my_order.my_order_cancel, name="my_order_cancel"),

    path('my/transaction/list/', my_transaction.my_transaction_list, name="my_transaction_list"),
    path('transaction/list/', my_transaction.transaction_list, name="transaction_list"),

]
```

**settings.py配置：**

```python
from pathlib import Path

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent

# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/3.2/howto/deployment/checklist/

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'django-insecure-9pt%1b%f2*&*)ipcfmzec&69r(e(q#(2m-$yx1m85g-4-pi%wz'

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = []

# Application definition

INSTALLED_APPS = [
    # 'django.contrib.admin',
    # 'django.contrib.auth',
    # 'django.contrib.contenttypes',
    # 'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'web.apps.WebConfig'
]

MIDDLEWARE = [
    # 'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    # 'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'utils.md.AuthMiddleware'
]

ROOT_URLCONF = 'day06.urls'

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
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'day06.wsgi.application'

# Database
# https://docs.djangoproject.com/en/3.2/ref/settings/#databases

DATABASES = {

}

# Password validation
# https://docs.djangoproject.com/en/3.2/ref/settings/#auth-password-validators

AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]

# Internationalization
# https://docs.djangoproject.com/en/3.2/topics/i18n/

# LANGUAGE_CODE = 'en-us'
LANGUAGE_CODE = 'zh-hans'

TIME_ZONE = 'UTC'

USE_I18N = True

USE_L10N = True

USE_TZ = True

# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/3.2/howto/static-files/

STATIC_URL = '/static/'

# Default primary key field type
# https://docs.djangoproject.com/en/3.2/ref/settings/#default-auto-field

DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'

MENU = {
    "ADMIN": [],
    "CUSTOMER": [],
}

PERMISSION = {
    "ADMIN": [],
    "CUSTOMER": [],
}

# cache缓存配置
CACHES = {

}

# session配置

SESSION_ENGINE = 'django.contrib.sessions.backends.cache'
SESSION_CACHE_ALIAS = 'default'
# 自定义cookie过期时间
SESSION_COOKIE_AGE = 60 * 60 * 24 * 7 * 2

#   自定义配置
LOGIN_HOME = "/home/"
MY_SESSION_KEY = "user_info"
MY_LOGIN_URL = "/login/"
MY_WHITE_URL = ["/login/", "/sms/login/", "/sms/send/"]

MY_MENU = {
    "ADMIN": [
        {
            'text': "用户信息",
            'icon': "fa-bed",
            'children': [
                {'text': "级别管理", 'url': "/level/list", 'name': "level_list"},
                {'text': "客户管理", 'url': "/customer/list", 'name': "customer_list"},
                {'text': "价格策略", 'url': "/policy/list", 'name': "policy_list"},
            ]
        },
        {
            'text': "交易管理",
            'icon': "fa-bed",
            'children': [
                {'text': "交易记录", 'url': "/transaction/list/", 'name': "transaction_list"},
            ]
        }

    ],
    "CUSTOMER": [
        {
            'text': "用户信息",
            'icon': "fa-keyboard-o",
            'children': [
                {'text': "订单管理", 'url': "/my/order/list", 'name': "my_order_list"},
                {'text': "我的交易记录", 'url': "/my/transaction/list/", 'name': "my_transaction_list"},
            ]
        }
    ],
}

MY_PPERMISSION_PUBLIC = {
    "home": {"text": "主页", "parent": None},
    "logout": {"text": "注销", "parent": None},
}

MY_PERMISSION = {
    "ADMIN": {
        "level_list": {"text": "级别列表", "parent": None},
        "level_add": {"text": "新建级别", "parent": 'level_list'},
        "level_edit": {"text": "编辑级别", "parent": 'level_list'},
        "level_delete": {"text": "删除级别", "parent": 'level_list'},

        "customer_list": {"text": "客户列表", "parent": None},
        "customer_add": {"text": "新建客户", "parent": 'customer_list'},
        "customer_edit": {"text": "编辑客户", "parent": 'customer_list'},
        "customer_reset": {"text": "重置密码", "parent": 'customer_list'},
        "customer_delete": {"text": "删除客户", "parent": 'customer_list'},
        "customer_charge": {"text": "我的交易记录", "parent": 'customer_list'},
        "customer_charge_add": {"text": "创建交易记录", "parent": 'customer_list'},

        "policy_list": {"text": "价格策略", "parent": None},
        "policy_add": {"text": "创建价格策略", "parent": 'policy_list'},
        "policy_edit": {"text": "编辑价格策略", "parent": 'policy_list'},
        "policy_delete": {"text": "删除价格策略", "parent": 'policy_list'},

        "transaction_list": {"text": "交易记录", "parent": None},

    },
    "CUSTOMER": {
        "my_order_list": {"text": "订单列表", "parent": None},
        "my_order_add": {"text": "新建订单", "parent": 'my_order_list'},
        "my_order_cancel": {"text": "订单", "parent": 'my_order_list'},
        "my_transaction_list": {"text": "我的交易记录", "parent": None},
    }
}

QUEUE_TASK_NAME = "YANG_TASK_QUEUE"
# MESSAGE_STORAGE = 'django.contrib.messages.storage.fallback.FallbackStorage'
# MESSAGE_STORAGE = 'django.contrib.messages.storage.cookie.CookieStorage'
MESSAGE_STORAGE = 'django.contrib.messages.storage.session.SessionStorage'

MESSAGE_DANGER_TAG = 50
MESSAGE_TAGS = {
    MESSAGE_DANGER_TAG: "danger"
}

import os

LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'file': {
            'level': 'ERROR',
            'class': 'logging.FileHandler',
            'filename': os.path.join(BASE_DIR, 'django_error.log'),
        },
    },
    'loggers': {
        'django': {
            'handlers': ['file'],
            'level': 'ERROR',
            'propagate': True,
        },
    },
}

try:
    from .local_settings import *
except ImportError:
    pass

```

**local_settings.py配置**：

> 放置数据库连接配置等线上与本地测试环境不同的设置
>

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'day06',  # 数据库名字
        'USER': 'root',
        'PASSWORD': 'Qiuwj1029',
        'HOST': '127.0.0.1',  # ip
        'PORT': 3306,
    }
}


# cache缓存配置
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



### 用户认证相关
#### 用户名密码登录
```python
form中各个字段放置的位置
form自定义字段中placeholder
raise ValidationError("用户名格式错误")
视图函数中为指定字段添加报错信息 form.add_error("username", "用户名或密码错误")
```

+ 访问`/login/`
+ 来到`account.py下的login()视图函数`



![](/post/Python/order_trading_platform/1732500920107-dd72d923-0813-43c5-9cc2-cbc1851acf06.png)

+ 用户看到login.html填写表单并提交，回到`account.py下的login()视图函数`

![](/post/Python/order_trading_platform/1732501766273-5e19ada5-1989-4867-b1db-5b2389279505.png)

![](/post/Python/order_trading_platform/1732501497237-59a80514-8caa-4035-984d-d8386fab982a.png)

#### 手机验证码登录
```python
模板中利用name反向生成url {% url 'login' %}
form中label属性
json返回值对象
发送验证码及对应页面效果实现的js
```

+ 点击短信登陆进行跳转

![](/post/Python/order_trading_platform/1732501594477-80367c51-4524-4f77-bf0f-fae47c252c31.png)

![](/post/Python/order_trading_platform/1732501621069-6f7e6a6c-bd40-4b07-a3ee-ee4b805e5cae.png)

+ 用户看到sms_login.html

![](/post/Python/order_trading_platform/1732502286562-c6bb7e54-0678-4fc8-8bb5-7995c3bef201.png)

+ 发送验证码前置

![](/post/Python/order_trading_platform/1732502965263-ae771897-7ce1-4d95-9db7-0e1d24fc6218.png)

+ 发送验证码

![](/post/Python/order_trading_platform/1732503252223-95871ea5-ce14-4d93-b339-4200302cf554.png)

+ 登录成功

![](/post/Python/order_trading_platform/1732503341745-241794bd-3215-4d53-bd86-88cff7951f3d.png)



### 菜单和权限
#### 访问权限
```python
process_request，基于他实现用户是否已登录，继续；未登录则返回登录界面。
	- return None，继续向后访问
	- return 对象，直接返回。
	
process_view，权限校验
	- return None，继续向后访问
	- return 对象，直接返回。
	- 在他的request对象中有 resolver_match  ，包含当前请求的视图路由信息  .name->sms_login
		admin = ['sms_login',"xxx"]
	
process_response
```

+ 是否已登录

```python
request中取出访问路径 request.path_info
白名单（登录页面）判断
从session中获取信息判断登录
登陆成功封装用户信息至request
```

![](/post/Python/order_trading_platform/1732583177967-d3c9f1f3-2401-4c56-b006-3f5f12d5cac7.png)

+ 菜单页面优化

layout.html 母版

```python
{% load static %}
{% load menu %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="{% static '/plugins/bootstrap/css/bootstrap.min.css' %}">
    <link rel="stylesheet" href="{% static '/plugins/font-awesome/css/font-awesome.min.css' %}">
    <link rel="stylesheet" href="{% static '/css/commons.css' %}">
    <link rel="stylesheet" href="{% static '/css/menu.css' %}">
    <link rel="stylesheet" href="{% static '/css/nav.css' %}">
    <link rel="stylesheet" href="{% static 'css/search-group.css' %}">
    {% block css %}{% endblock %}
</head>
<body>

<div class="pg-header">
    <div class="nav">
        <div class="logo-area left">
            <a href="{% url 'home' %}">
                <span style="font-size: 18px;">MY的管理平台</span>
            </a>
        </div>
        <div class="right-menu right clearfix">

            <div class="user-info right">
                <a href="#" class="avatar" style="text-decoration: none;">
                    <span style="color: white;margin-right: 5px;">{{ request.my_user.name }}</span>
                    <img class="img-circle" style="width: 35px;height: 35px;" src="{% static 'images/default.png' %}">
                </a>

                <div class="more-info">
                    <a href="{% url 'logout' %}" class="more-item">注销</a>
                </div>
            </div>
        </div>
    </div>

</div>

<div class="pg-body">
    <div class="left-menu">
        <div class="menu-body">
            {% my_menu request %}
        </div>
    </div>
    <div class="right-body">
        <ol class="breadcrumb">
            {% for text in request.my_user.text_list %}
                <li><a href="#">{{ text }}</a></li>
            {% endfor %}
        </ol>
        <div style="padding: 20px">
            {% block content %}{% endblock %}
        </div>

    </div>
</div>

<script src="{% static 'js/JQuery3.6.0.js' %}"></script>
<script src="{% static 'plugins/bootstrap/js/bootstrap.min.js' %}"></script>
<script src="{% static 'js/menu.js' %}"></script>
{% block js %}{% endblock %}

</body>
</html>
```

home.html

> 只需继承layout.html并拓展即可
>

```python
{% extends 'layout.html' %}

{% block content %}
    <h3>欢迎</h3>

{% endblock %}
```

+ 动态菜单

![](/post/Python/order_trading_platform/1732598117149-f4ddfbba-a2d3-48e7-af1d-4f0f0453756d.png)

+ 顶部导航（注销）

![](/post/Python/order_trading_platform/1732598564449-30c0f304-0cac-4e20-83ca-27421423c926.png)

+ 权限校验

![](/post/Python/order_trading_platform/1732694957748-4f79bd97-c777-4c4e-b154-f56c7a30aa15.png)

+ 顶部导航栏与关联菜单选中样式

![](/post/Python/order_trading_platform/1732695541151-0b5c3516-7df2-411b-9539-26e85b61cda7.png)

![](/post/Python/order_trading_platform/1732695642676-d757fb34-e278-4b92-9314-dd73eeed9874.png)

#### 按钮显示权限
```python
check_permission:有无权限
has_permission:用于控制页面的显示，有对应权限则展示页面
其他方法:根据传入的url的name与参数反向生成url
编辑处需要处理跳转时的参数携带问题
```

```python
from django.http import QueryDict
from django.template import Library
from django.conf import settings
from django.urls import reverse
from django.utils.safestring import mark_safe

register = Library()


def check_permission(request, name):
    # print(request, name, args, kwargs)
    # <WSGIRequest: GET '/customer/list/'> customer_add () {}
    # 1.获取当前登录用户角色
    role = request.my_user.role

    # 2.根据角色获取权限字典
    permission_dict = settings.MY_PERMISSION[role]
    # 3.判断权限
    if name in permission_dict:
        return True
    if name in settings.MY_PPERMISSION_PUBLIC:
        return True


@register.simple_tag
def add_permission(request, name, *args, **kwargs):
    # 3.1无权限，返回空
    if not check_permission(request, name):
        return ""

    # 3.2有权限，通过name反向生成URL
    url = reverse(name, args=args, kwargs=kwargs)

    tpl = """<a href="{}" class="btn btn-success">
        <span class="glyphicon glyphicon-plus-sign"></span>新建
    </a>""".format(url)

    return mark_safe(tpl)


@register.simple_tag
def edit_permission(request, name, *args, **kwargs):
    # 3.1无权限，返回空
    if not check_permission(request, name):
        return ""

    # 3.2有权限，通过name反向生成URL
    url = reverse(name, args=args, kwargs=kwargs)
    # 根据当前用户请求获取GET参数
    param = request.GET.urlencode()  # page=9&xx=12
    if param:
        new_query_dict = QueryDict(mutable=True)
        new_query_dict['_filter'] = param
        filter_string = new_query_dict.urlencode()
        tpl = """<a href="{}?{}" class="btn btn-primary btn-xs">编辑</a>""".format(url, filter_string)
        return mark_safe(tpl)
    tpl = """<a href="{}" class="btn btn-primary btn-xs">编辑</a>""".format(url)
    return mark_safe(tpl)


@register.simple_tag
def delete_permission(request, name, *args, **kwargs):
    # 3.1无权限，返回空
    if not check_permission(request, name):
        return ""

    # 3.2有权限，通过name反向生成URL
    pk = kwargs.get('pk')
    tpl = """<a cid="{}" class="btn btn-danger btn-xs btn-delete"">删除</a>""".format(pk)
    return mark_safe(tpl)


@register.simple_tag
def delete_url_permission(request, name, *args, **kwargs):
    # 3.1无权限，返回空
    if not check_permission(request, name):
        return ""

    url = reverse(name, args=args, kwargs=kwargs)
    tpl = """<a href="{}" class="btn btn-danger btn-xs btn-delete">删除</a>""".format(url)
    return mark_safe(tpl)


@register.filter
def has_permission(request, others):
    name_list = others.split(',')
    for name in name_list:
        if check_permission(request, name):
            return True
    return False

```



### 级别管理
![](/post/Python/order_trading_platform/1732700979200-7e6e5546-7dee-4532-bffa-ff9f314e549f.png)

#### 级别列表
```python
如果用户没有对应权限就不要显示按钮
```

![](/post/Python/order_trading_platform/1732701720064-3a58477a-81ee-41b7-8449-593d4665076a.png)



#### 新增级别
![](/post/Python/order_trading_platform/1732708539130-2b8bfdc2-487c-4d32-8f9e-8e5440e05d2c.png)

#### 编辑级别
![](/post/Python/order_trading_platform/1732708762614-b6b7596c-1305-428e-82b3-5e979cb5c4a6.png)

#### 删除级别
![](/post/Python/order_trading_platform/1732709150253-189c3fac-8b69-4f92-889a-a3312cf450b2.png)





### 客户管理
![](/post/Python/order_trading_platform/1732709272761-5737ab89-656d-499f-8141-2793e6cb4862.png)



#### 客户列表 
```python
读取客户列表页面展示
问题： 如果关联的级别被删除该怎么处理？
	1.删除级别时判断是否有下属客户，有则不允许删除
	2.自动将级别设为某个级别
	3.不做任何处理，后续客户查询校验level_active=1
模板的时间格式
级别处显示对象而非level.title
	1.主动连表查询
	2.models中重写__str__方法指定输出
```

视图函数

![](/post/Python/order_trading_platform/1732709842539-afbc14bf-f6bd-4dae-9dd7-cdd0cfa26063.png)

模板

![](/post/Python/order_trading_platform/1732710156474-b8061e66-cbe8-4ef2-8f27-cc11bde7e842.png)

![](/post/Python/order_trading_platform/1732710607268-8e661743-6b94-4bd9-bfbb-4f9d5a458a45.png)



#### 新建客户
```python
新增form2.html模板，一行填写两个字段（栅栏样式）
新增重复密码字段
展示级别条件筛选（active=1）
	1.models中limit_choice_to
	2.自定义choices和queryset
展示级别管理-页面插件（select -> radio)
	1.局部去除bootstrap样式
	2.修改下拉框为radio框
提交保存
	1.models表单验证，正则
	2.自定义钩子，保证密码一直
	3.models添加validators
	4.保存（手动添加creator字段）
	5.跳转回页面
```

![](/post/Python/order_trading_platform/1732710963113-314dc788-1681-43e2-9691-c4a183b4f5d9.png)

#### 编辑客户
```python
和添加的页面不同，新建一个form
```

![](/post/Python/order_trading_platform/1732711225774-04e3868a-9b09-4708-aaf3-52854942af3d.png)

#### 重置密码
![](/post/Python/order_trading_platform/1732711342983-96fffb37-2bfa-4a82-9f27-951a787cc325.png)



#### 删除客户（对话框和ajax）
```python
模态对话框的触发方式
	1.data-target属性
	2.绑定点击函数
	3.一键绑定
删除逻辑实现
删除成功后页面处理
```

![](/post/Python/order_trading_platform/1732711493928-3f4e65ca-4047-407d-84b2-5231701ab74f.png)



#### 交易记录列表
视图函数

![](/post/Python/order_trading_platform/1732712848306-c24732ba-3439-4ea0-b420-d480c5ca046a.png)

模板

充值扣款添加颜色

![](/post/Python/order_trading_platform/1732713412511-d3341d19-c1d6-409a-96ac-a5a82e8411ed.png)

![](/post/Python/order_trading_platform/1732713612510-d8e3a230-7f2d-489a-9f70-fd4e2b088751.png)

#### 充值和扣款（对话框）
模板

#### ![](/post/Python/order_trading_platform/1732714253476-a85ee918-3e85-4bc0-9420-fb090b70999e.png)
视图函数

![](/post/Python/order_trading_platform/1732714421549-44c80ea4-589a-4a96-ad9d-da9030ba1ddd.png)

### 价格策略
![](/post/Python/order_trading_platform/1732714728946-9c197fdf-ae64-4434-ba7b-09a3ef0dc039.png)

#### 价格策略列表
![](/post/Python/order_trading_platform/1732717257949-47129a3d-c95d-40fb-b862-0417156dd829.png)

#### 新建价格策略
![](/post/Python/order_trading_platform/1732717308086-c7ff3823-ce95-4f3a-9a53-22d7198b86e5.png)

#### 编辑价格策略
![](/post/Python/order_trading_platform/1732717332909-5f7038ac-971f-4617-a7f6-cd389653e85b.png)

#### 删除价格策略
![](/post/Python/order_trading_platform/1732717354155-23522bb7-dd9a-4e67-ad94-246b54868123.png)



### 订单管理（用户）
+ 我的订单列表
+ 下单
    - 输入：视频地址、数量（提示价格策略）
    - 数据库操作（事务 + 锁）
        * 创建订单记录
        * 创建交易记录
        * 扣款
    - 写入redis队列（等待执行）
+ 撤销订单（待执行）
    - 数据库操作
        * 更新订单状态
        * 生成交易记录
        * 归还扣款

![](/post/Python/order_trading_platform/1732717529916-4ea32a05-7e7e-4c35-9eb6-9721c26572bb.png)

#### 我的订单列表
![](/post/Python/order_trading_platform/1732718252013-ebb91e8a-6f34-497c-865f-c6d0715ae7bf.png)

message

#### 新建订单
![](/post/Python/order_trading_platform/1732718673191-5cf5603c-69fe-477c-9d2f-283a4b6d1237.png)

![](/post/Python/order_trading_platform/1732718836457-70e387e0-141e-4175-8e55-25a7363eb655.png)

#### 撤销订单
![](/post/Python/order_trading_platform/1732721178721-88384401-a19f-446d-9661-d957c2b2c46d.png)



### 我的交易记录
![](/post/Python/order_trading_platform/1732721310752-098e3751-9820-412c-a3ef-8a2e8a511fed.png)



#### 客户
![](/post/Python/order_trading_platform/1732721442476-eef3795d-2b96-4160-8345-2d9c8ba09d53.png)

![](/post/Python/order_trading_platform/1732721550932-6d819b16-6653-4b80-b620-9b6d40c1af7d.png)

#### 管理员
![](/post/Python/order_trading_platform/1732721666199-d74a8162-588c-4425-85b4-32366ad50ab1.png)

模板与客户无异



### Worker
![](/post/Python/order_trading_platform/1732721732393-6b533304-9dd0-41fa-b251-04ab2f9b2bcb.png)

执行worker去执行订单的执行。
