---
title: "Vue2"
slug: "vue2"
description: "介绍Vue2的相关知识"
date: "2025-02-27T19:08:20+08:00"
lastmod: "2025-02-27T19:08:20+08:00"
image: 
categories: ["前端"]
tags: ["Vue"]
---
这是摘要

<!--more-->

## Vue介绍

### 前端发展史



```python
# 1 html(5),css(3),javascript=(ECMA:5，补充es6语法,最新:13，bom:浏览器对象,dom:文档对象)--> 编写一个个的页面 -> 给后端(PHP、Python、Go、Java) -> 后端嵌入模板语法(dtl:django的模版语法 xx.html ) -> 后端渲染完数据 -> 返回数据给前端 -> 在浏览器中查看
	-es6更新内容 箭头函数，模块导入，let
    -模版语法：例如django的模板语法，在index.html中写了python语法 {%  %}---》模版语法
    	- 在{% %}中写入python语法的表达式，python解释器负责把python语法解析完转成 纯粹的html，css，js
   
  
# 2 Ajax的出现 -> 后台发送异步请求，Render+Ajax混合

# 3 单用Ajax（加载数据，DOM渲染页面）：前后端分离的雏形

# 4 Angular框架的出现（1个JS框架）：出现了“前端工程化”的概念（前端也是1个工程、1个项目）：笨重，越来越少的人用了

# 5 React、Vue框架：当下最火的2个前端框架（Vue：国人写的，国人喜欢用，中小厂，全栈工程师，React：外国人喜欢用，大厂喜欢用）

# 6 移动开发（Android+IOS） + Web（Web+微信小程序+支付宝小程序） + 桌面开发（Windows桌面）：前端 -> 大前端

# 7 可不可以一套编码，多端运行 ：一套代码在各个平台运行（大前端）：谷歌Flutter平台（Dart语言：和Java很像）可以运行在IOS、Android、PC端，桌面端支持不好---》部分公司在用，坑比较多

# 8 在Vue框架的基础性上 uni-app：一套编码 编到10个平台----》学习vue的性价比很高

# 9 在不久的将来 ，前端框架可能会一统天下
 
  
 
# ts和es：
    typescript：微软出的脚本语言，强类型，微软开发的一个开源的编程语言，通过在JavaScript的基础上添加静态类型定义构建而成
    ecma：标准---》js语言，弱类型
    typescript是js的超集
    写前端可以使用js，也可以使用ts，ts兼容js的
    js：历史遗留了很多坑，未来可能会被替代

  
大前端时代：html,css,js--->PC端(网页)，移动端（安卓，ios）,小程序(微信官方:标签是html5内容)
```

时期1

![image-20221016094526430](/post/Vue/Vue2/image-20221016094526430.png)

时期2

![image-20221016094955593](/post/Vue/Vue2/image-20221016094955593.png)





### Vue介绍

Vue 是一个 js 框架(jquery也是js框架)，同时也是一个渐进式框架(单个页面使用vue或者整个项目都使用vue)

- 是一套用于构建用户界面的渐进式框架
- 与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用
- Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合

官网：https://cn.vuejs.org/

 Vue 版本问题：两个版本（些许差距，基础都是一样的）

- 2.x版本：老版本
- 3.x版本：完整支持ts语法,2020年至今
- vue： 正处于转型期，大量公司还在用vue3，少量公司在用vue3

特点：

- 通过 HTML、CSS、JavaScript构建应用--》要有这些东西基础
- 不断繁荣的生态系统，可以在一个库和一套完整框架之间自如伸缩
- 20kB min+gzip    运行大小    超快虚拟 DOM    最省心的优化

M-V-VM思想

- django:MTV   其他的一些后端框架mvc架构
- MVP：移动端会使用
- Vue：M-V-VM 架构
  - Model ：vue对象的data属性里面的数据，这里的数据要显示到页面中，js里面有变量
  - View ：vue中数据要显示的HTML页面，在vue中，也称之为“视图模板” （HTML+CSS）
  - ViewModel：vue中编写代码时的vm对象，它是vue.js的核心，负责连接 View 和 Model数据的中转，保证视图和数据的一致性，所以前面代码中，data里面的数据被显示在p标签中就是vm对象自动完成的（双向数据绑定：JS中变量变了，HTML中数据也跟着改变）

以后不需要咱们操作dom了---> 因为有vm可以帮我们监听Model数据变化，自动改变View 

jquery选择器，原生js 完全用不到了---> jq已经完成自己的历史使命了，慢慢退出舞台了



组件化开发：组件化开发：把某些可能会多次使用的 htm，css，js，写成单独的，以后直接引入即可

单页面开发：如果是vue项目，整个项目其实就一个 index.html 页面，看到的页面变化，不是多个html页面在变化，而是组件的替换

![image-20221016103403145](/post/Vue/Vue2/image-20221016103403145.png)

## Vue的快速使用

```python
# 补充：工欲善其事必先利其器  ---> 咱们要开发vue，需要有一个比较趁手 集成开发工具(IDE)
	-vscode:微软出的，不仅可以写前端，还可以写python,go.....  免费的
    -Jetbrains捷克的公司公司出的：全家桶：pycharm，goland，IDEA(java)，phpstorm，webstorm(前端)。。。
  		-收费，挺贵  全家桶的授权：599刀一年  单版本授权 199刀 一年
    	-破解：pycharm本身跟webstorm师出同门
    -直接使用Pycharm--->vue的插件，就可以写vue了---->讲课用，不需要再多下一个编辑器
    -直接下载webstorm-->破解方案跟pycharm完全一样
    
# 使用Pycharm开发--->下载后有vue代码提示，不下也能开发vue
    -新建一个项目（python项目也可以）
  	-装一个vue的插件：setting中搜plugins--->vue--->安装--->ok--->apply

# Vue 就是一个js框架----》把源代码下载下来(cdn)，引入到页面中去
	-先用vue2的讲：基础内容是一样的
    -拿到vue2的源码，放在咱们工程中--》js文件夹下vue.js

	-渐进式框架---》在一个html中使用vue---》跟之前使用jq是一模一样
```

第一步：创建项目

- 直接和新建一个python项目一样，没区别，且不用管python解释器

第二步：安装Vue插件（提供代码提示，更方便开发，不下也可以开发Vue）

![image-20250124144106255](/post/Vue/Vue2/image-20250124144106255.png)

第三步：Vue导入本地（如果采用远程导入如cdn，则不需要这一步）

因为Vue就是一个框架，和jquery一样，我们都需要把源代码下载下来，引入到页面中

- Vue.js v2.7.16版本的cdn地址 `https://cdn.jsdelivr.net/npm/vue/dist/vue.js` 
- 将上述地址的源码复制到项目目录下的`js/vue.js`文件

![image-20250124145430369](/post/Vue/Vue2/image-20250124145430369.png)

第四步：开发第一个vue项目

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <!-- 引入vue,跟之前引入jquery是一样的-->
    <script src="./js/vue.js"></script>
    <title>标题</title>
</head>
<body>

<div id="app">
    <!--以后在这div内部，就可以写vue的语法了,插值语法，把变量放在{{}}内，它就会把变量渲染在这-->
    <h1>{{name}}</h1>
</div>
</body>

<script>
    // 写js代码,new实例化得到一个vue对象,传入配置项，用{}包裹的是js中的一个对象，有key，有value的格式
    new Vue({
        el: '#app', // 根据id 找到div，这个div就被vue托管了，以后div中就可以写vue的语法了
        data: {
            name: 'lqz'  // 定义了一个变量 name
        }
    })

</script>

</html>
```



## 文本插值

文本插值（Text Interpolation）是通过双大括号 `{{ }}` 实现的，用于将 Vue 实例的数据动态插入到 HTML 模板中。文本插值可以插入的内容类型非常灵活，可以插入变量，函数和短的表达式



补充说明一下：

```python
python 字典      {'name':'lqz','age':19}
js 对象		  {name:"lqz",age:19}

python和js都支持将其对应的字典和对象转换为json字符串
json 格式：字符串   {"name":"lqz","age":19}
```

js的对象中，key可以不用引号包裹，这是es6的新语法，旨在开发者少写代码



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./js/vue.js"></script>
    <title>标题</title>
</head>
<body>

<div id="app">
    <h1>渲染变量</h1>
    <p>我的名字:{{name}}</p>
    <p>我的年龄:{{age}}</p>
    <p>数组:{{l}}</p>
    <p>数组取值:{{l[1]}}</p>
    <p>对象:{{obj}}</p>
    <p>对象取值:{{obj['age']}}</p>
    <p>对象取值:{{obj.name}}</p>
    <p>标签：{{s}}</p>

    <h1>渲染函数--后面讲</h1>

    <h1>渲染表达式：三目运算符,一定不要写太长</h1>
    <p>{{age > 20 ? '男人' : '男孩'}}</p>
    <p>{{age == 19}}</p>


</div>
</body>

<script>
    new Vue({
        el: '#app',
        data: {
            name: 'lqz',
            age: 19,
            l: [1, 2, 3], // js中叫数组，python中叫列表
            obj: {name: '彭于晏', age: 40},  // js中叫对象，python中叫字典  对象取值不加引号，会将key值当成一个变量
            s: '<input type="text">',  // 字符串
            t: 10 > 9 ? '大于' : '小于'  // 跟python中的三元表达式是一样的  t= 条件 ? '符合条件':'不符合'
            // t:'大于'

        }
    })

</script>

</html>
```

显示结果：

![image-20250125005745293](/post/Vue/Vue2/image-20250125005745293.png)



## 指令

指令是什么？

放在标签上,以v-开头的，称之为指令，每个指令都能完成相应操作，例如：

`<p v-html='js变量，函数，表达式'> </p>`

下面我们来具体学习不同的指令有什么功能



### 文本指令（操作文本相关）

v-html：把变量的内容当html渲染到标签中去（变量是一个字符串，包裹的内容是标签，会被完整渲染为标签）

v-text:把变量内容当字符串写到到标签中（变量是一个字符串，包裹的内容是标签，不会渲染成标签，而是文本内容），如果原来标签内文本部分有内容，会被清空

<!--可以完成跟 {{}} 一样的功能，但是还是不一样的，他是放在属性部分，而插值语法是在标签内部-->

v-show：只能跟 true  或 false  或   表达式运算结果是布尔类型，控制标签的显示与否

 v-if ：只能跟 true  或 false  或   表达式运算结果是布尔类型，控制标签的显示与否



vue中面试题：v-if和v-show区别是什么？

- v-if:如果是false，不显示，标签也不存在（需要删除和添加标签，效率低）
- v-show,如果是false，不显示，但是标签存在，只是通过样式 dispaly=none控制的

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./js/vue.js"></script>
    <title>标题</title>
</head>
<body>


<div id="app">
    <h1>v-html</h1>
    <p>{{s}}</p>
    <p v-html="s"></p>
    <div v-html="s"></div>

    <h1>v-test</h1>
    <p v-text="s">美女</p>

    <h1>v-show和v-if</h1>
    <div v-show="b2">美女</div>
    <div v-if="b1">帅哥</div>

    <div v-show="b1">美女</div>
    <div v-if="b2">帅哥</div>

</div>
</body>

<script>
    new Vue({
        el: '#app',
        data: {
            s: '<input type="text">',
            b1: true,
            b2: false
        }
    })

</script>

</html>
```

**显示效果**

![image-20250125012522198](/post/Vue/Vue2/image-20250125012522198.png)

### 补充小知识：MVVM的双向数据绑定

双向数据绑定：model的数据变，view也跟着变；只要view变化，model也跟着变化

```html
var vm = new Vue({
    el: '#app',
    data: {
        s: '<input type="text">',
        b1: true,
        b2: false
    }
})
```

把实例化后的Vue对象赋值给变量vm，以后从vm中可以通过 vm._data 取到 实例化的时候的data，也可以从vm.变量名取到data中的变量

在控制台修改变量的值，页面会立马跟着变化 ---> 以后只需要修改变量值即可，不需要操作dom了

![image-20250125013904332](/post/Vue/Vue2/image-20250125013904332.png)

如图，直接修改`vm.b2=true`，就可以显示出原先被隐藏的内容，可见，修改Model即可修改View

后端数据库的值改变并不会引起页面变化，只有通过ajax请求到数据后，前端将数据给到Model，Model的变化才会引起View的变化

![image-20221016142247104](/post/Vue/Vue2/image-20221016142247104.png)

那么View变化是否也会引起Model改变呢？

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./js/vue.js"></script>
    <title>标题</title>
</head>
<body>


<div id="app">
    <h1>v-html</h1>
    <p>{{s}}</p>
    <p v-html="s"></p>
    <div v-html="s"></div>

    <h1>v-test</h1>
    <p v-text="s">美女</p>

    <h1>v-show和v-if</h1>
    <div v-show="b2">美女</div>
    <div v-if="b1">帅哥</div>

    <div v-show="b1">美女</div>
    <div v-if="b2">帅哥</div>

    <input type="text" v-model="name">


</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            s: '<input type="text">',
            b1: true,
            b2: false,
            name: ''
        }
    })

</script>

</html>

```

创建一个空的input框，当在页面上填入内容时，name也相应发生变化

![image-20250125014344980](/post/Vue/Vue2/image-20250125014344980.png)

至此我们已经看到了Vue的双向数据绑定



### 事件指令

v-on：给标签绑定一个事件，当触发这个事件，就会执行函数

语法：`v-on:事件名='函数'`，可以简写成`@事件名='函数'`（常用）

```html
<button class="btn btn-danger" v-on:click="handleClick">点我看美女</button>
<button class="btn btn-danger"  @click="handleClick">点我看美女2</button>
```

补充一下关于前端各种技术的介绍

```python
-html：标签
-css：样式（好看）  bootstrap是css样式，element-ui
-js：动起来        jquery是js框架，vue是js框架，react
```

在给出函数定义的代码前，先补充一个关于es6语法中对象的写法

```python
es6 语法的对象写法
    var obj = {'name': 'lqz', 'age': 19}
    var obj1 = {name: 'lqz', age: 19}

    name = 'lqz'
    age = 19
    var onj2 = {name, age}  // 等同于{name: 'lqz', age: 19}  es6 对象写法
```

由此函数的定义就可以由方式一简化到方式二

```html
<script>
    var vm = new Vue({
        el: '#app',
        // 变量写在data中
        data: {
            zs:true,
        },
        // 函数写在methods中
        methods: {
            // 方式一：es5的语法
            // handleClick: function () {
            //     alert('美女')
            // }
            // 方式二：常用的es6的语法
            handleClick() {
                alert('美女')
            },
            handleShow(){
                // 修改show的值为  !show
                // this代指的是vm对象，Vue对象
                this.zs=!this.zs
            }
        }, // 函数写在这里
    })
</script>
```

完整代码及案例

> 实现通过一个按钮控制一张图片的显示状态的切换

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="https://cdn.bootcss.com/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">

    <script src="./js/vue.js"></script>
    <title>标题</title>
</head>
<body>


<div id="app">

<!--    <button class="btn btn-danger" v-on:click="handleClick">点我看美女</button>-->
    <button class="btn btn-danger"  @click="handleClick">点我看美女2</button>
    <br>



<!--    写个按钮，一点击就显示美女图片，再点击一次就不显示了-->
    <button @click="handleShow">看我美女显示和消失</button>
    <br>
<!--    <img v-if="zs" src="https://tva1.sinaimg.cn/large/00831rSTly1gd1u0jw182j30u00u043b.jpg" alt="" height="500px" width="500px">-->
    <img v-show="zs" src="https://tva1.sinaimg.cn/large/00831rSTly1gd1u0jw182j30u00u043b.jpg" alt="" height="500px" width="500px">

</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        // 变量写在data中
        data: {
            zs:true,
        },
        // 函数写在methods中
        methods: {
            // 方式一：es5的语法
            // handleClick: function () {
            //     alert('美女')
            // }
            // 方式二：常用的，es6的语法
            handleClick() {
                alert('美女')
            },
            handleShow(){
                // 修改show的值为  !show
                // this代指的是vm对象，Vue对象
                this.zs=!this.zs

            }

        },
    })


</script>

</html>
```



### 属性指令

标签会有很多属性，比如

```html
<a href=""></a>   
<img src="" >  
<p name="xxx">
</p> <button class="btn">点我</button>
```

v-bind：可以将标签的任意属性动态的绑定为一个变量

语法：`v-bind:属性名="变量"`可以简写为`:属性='变量'`

```html
<img v-bind:src="url" alt="" width="500px" height="500px">
<img :src="url" alt="" width="500px" height="500px">
```

案例：点击按钮实现图片的切换（改变img标签的src属性）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./js/vue.js"></script>
    <title>标题</title>
</head>
<body>


<div id="app">

    <!--    <img v-bind:src="url" alt="">-->

    <h1>点我随机弹美女图片</h1>
    <button @click="handleClick">点我</button>
    <br>
<!--    <img v-bind:src="url" alt="" width="500px" height="500px">-->
    <img :src="url" alt="" width="500px" height="500px">


</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            url: 'https://tva1.sinaimg.cn/large/00831rSTly1gd1u0jw182j30u00u043b.jpg',
            l: ['https://img2.woyaogexing.com/2022/10/13/05e406595e217e0c!400x400.jpg',
                'https://img2.woyaogexing.com/2022/10/13/c03408591b068c18!400x400.jpg',
                'https://img2.woyaogexing.com/2022/10/13/f810d999375f7f1c!400x400.jpg']
        },
        methods: {
            handleClick() {
                // 随机从生成0--3的数字
                this.url = this.l[Math.floor(Math.random()*3)];
            }
        }
    })

</script>

</html>
```



### style和class（特殊的属性指令）

属性指令中，style和class属性特殊一点，因此单独拿出来讲，绑定语法与属性指令相同，只是被绑定的内容特殊

class属性可以绑定：字符串，数组（常用），对象

- 常用数组，因为数组的结构与class属性相似，有多个class属性值，且数组方便实现添加与删除

```javascript
// 字符串
class_str: 'red size',
// 数组
class_list: ['red', 'size'],
// 对象
class_obj: {red: true, size: false, back: false},
```

style 属性可以绑定： 字符串，数组，对象（常用）

- 常用对象，因为style属性原本就键值的形式，与对象类型相似

```javascript
// 字符串
style_str: 'font-size: 50px;background: pink',
// 数组
style_list: [{'font-size': '50px'}, {'background': 'pink'}, {'color': 'green'}],
// 数组写法二，如果是 - 相连的两个单词,当键省略引号时，需要更改为驼峰形式
style_list: [{fontSize: '50px'}, {background: 'pink'}, {color: 'green'}], 
// 对象
style_obj: {fontSize: '50px', background: 'pink', color: 'green'},
```

案例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./js/vue.js"></script>
    <title>标题</title>
    <style>
        .red {
            color: crimson;
        }

        .back {
            background: greenyellow;
        }

        .size {
            font-size: 50px;
        }

    </style>
</head>
<body>


<div id="app">
    <h1>class的三种绑定方式</h1>
    <!--    <div :class="class_str">-->
    <!--    <div :class="class_list">-->
    <div :class="class_obj">
        我是一个div
    </div>

    <hr>
    <h1>style的三种绑定方式</h1>
    <!--    <div style="font-size: 50px;background: pink;color: green">-->
    <!--    <div :style="style_str">-->
    <!--    <div :style="style_list">-->
    <div :style="style_obj">
        我是第二个div
    </div>

</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            // class:字符串，数组(常用，class由多个类组成，数组跟它最搭)，对象
            class_str: 'red size',
            class_list: ['red', 'size'], // 如果咱么向数组末尾最近一个类名，div样式就变了
            class_obj: {red: true, size: false, back: false},
            // style :字符串，数组，对象
            style_str: 'font-size: 50px;background: pink',
            style_list: [{'font-size': '50px'}, {'background': 'pink'}, {'color': 'green'}],
            // style_list: [{fontSize: '50px'}, {background: 'pink'}, {color: 'green'}], // 跟上面一样，如果是 - 相连的两个单词可以改成驼峰形式
            style_obj: {fontSize: '50px', background: 'pink', color: 'green'}, // style 来讲，它用的多
        },
        methods: {}
    })

</script>

</html>
```

同样的，在浏览器控制台修改class_list或style_obj的内容可以达到修改View的效果

## 条件渲染

也是一个指令：实现符合某个条件，就显示某个标签

和python中的条件语句相似，在python中时if，elif，else

而在vue中是`v-if，v-else-if，v-else`

案例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./js/vue.js"></script>
    <title>标题</title>
    <style>


    </style>
</head>
<body>


<div id="app">

    <p v-if="score>90">优秀</p>
    <p v-else-if="score>70 && score<=90">良好</p>
    <p v-else-if="score>60 && score<=70">及格</p>
    <p v-else>不及格</p>

</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            score: 98,
        },
    })

</script>

</html>
```

## 列表渲染

也是一个指令：实现循环渲染，相当于python中的for循环

`v-for`指令

- 可以循环数组   v-for="(url,index) in urls"      第一个变量得到的是值，第二个变量得到其下标
- 可以循环对象   v-for="(value,key) in obj"     第一个变量得到value，第二个变量key， 如果只用一个变量接收，则是value
- 可也循环数字   v-for="i in num"       从1开始，循环到数字本身

<!--注意，此处的循环对象与数字与python中循环字典和数字均有不同，注意区分-->

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./js/vue.js"></script>
    <title>标题</title>
    <style>


    </style>
</head>
<body>


<div id="app">
    <h1>把所有美女，一行行显示在页面中</h1>

    <ul>
        <!--        <li v-for="url in urls">-->
        <li v-for="(url,index) in urls">
            <h3>第{{index + 1}}张美女</h3>
            <img :src="url" alt="" height="100px" width="100px">
        </li>
    </ul>

    <h1>循环对象</h1>
<!--    <p v-for="(value,key) in obj">key值为：{{key}},value值为：{{value}}</p>-->
    <p v-for="value in obj">value值为：{{value}}</p>


    <h1>循环数字</h1>
    <p v-for="i in num">循环到了第：{{i}}</p>

</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            urls: ['https://img2.woyaogexing.com/2022/10/13/05e406595e217e0c!400x400.jpg',
                'https://img2.woyaogexing.com/2022/10/13/c03408591b068c18!400x400.jpg',
                'https://img2.woyaogexing.com/2022/10/13/f810d999375f7f1c!400x400.jpg'],
            obj: {name: 'lqz', age: 19, bobby: '篮球'} ,  // hash类型  hashmap  map  字典  对象 都是没有顺序
            num:4

        },
    })

</script>

</html>
```

页面效果

![image-20250126003133920](/post/Vue/Vue2/image-20250126003133920.png)

使用v-if和v-for写个购物车显示与不显示的案例

补充：

> == 比较值是否一样
> === 比较类型和值是否一样

v-if判断购物车是否为空，非空显示数据，为空显示文字说明

v-for在表格内循环生成商品内容

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./js/vue.js"></script>
    <link href="https://cdn.bootcss.com/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">

    <title>标题</title>

</head>
<body>


<div id="app">
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-6 col-md-offset-3">
                <div v-if="!good_list.length==0">
                    <table class="table table-striped table-bordered">
                        <thead>
                        <tr>
                            <th>商品id号</th>
                            <th>商品名字</th>
                            <th>商品价格</th>
                            <th>商品数量</th>
                        </tr>
                        </thead>
                        <tbody>
                        <tr v-for="good in good_list">
                            <th scope="row">{{good.id}}</th>
                            <td>{{good.name}}</td>
                            <td>{{good.price}}</td>
                            <td>{{good.num}}</td>
                        </tr>

                        </tbody>

                    </table>
                </div>
                <div v-else>
                    空空如也
                </div>

                <hr>
                <button @click="handleClick">点我刷新购物车</button>
            </div>

        </div>


    </div>


</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            good_list: []
        },
        methods: {
            handleClick() {
                // 跟后端交互，拿到了数据
                this.good_list = [
                    {id: 1, name: '小汽车', price: 100000, num: 1},
                    {id: 2, name: '钢笔', price: 6, num: 10},
                    {id: 3, name: '脸盆', price: 12, num: 6},
                    {id: 4, name: '水杯', price: 66, num: 5},
                ]

            }
        }
    })

</script>

</html>
```





## 数据的双向绑定

`v-model`指令，主要针对input标签，其他标签一般不用

input标签有多种类型，如`text，password，select`

我们先来讲text类型的input框的数据双向绑定

当我们使用`:value='变量'`时，是单向的数据绑定，即页面变化时，变量不会跟着变化。因此一般不用它

要实现数据的双向绑定，就需要使用`v-model='变量'`

```python
<input type="text" v-model="v">
```

案例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./js/vue.js"></script>
    <link href="https://cdn.bootcss.com/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">

    <title>标题</title>

</head>
<body>


<div id="app">

    <h1>v-model 只针对于input</h1>
    请输入内容：<input type="text" v-model="v"> ---->{{v}}


</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            v: '美女'
        },
    })

</script>

</html>
```



## 事件处理

### 常见事件

常见的事件有click点击事件，dbclick双击事件，blur失去焦点，input内容变化触发，change内容发生变化，且失去焦点触发

在input标签中主要是blur，input，change这三个事件

下面是一个案例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./js/vue.js"></script>

    <title>标题</title>

</head>
<body>


<div id="app">

    <h1>失去焦点事件blur</h1>
    <!--    请输入名字：<input type="text" v-model="name" @blur="handleBlur">-->
    <h1>change事件：数据发生变化才会触发</h1>
    <!--    请输入名字：<input type="text" v-model="name" @change="handleChange">-->
    <h1>input事件：只要输入内容会触发</h1>
    请输入名字：<input type="text" v-model="name" @input="handleInput">


</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            name: '彭于晏'
        },
        methods: {
            handleBlur() {
                alert(this.name)
            },
            handleChange() {
                console.log(this.name)  // 跟python中print一样
            },
            handleInput(){
                console.log(this.name)
            }
        }
    })

</script>

</html>
```

### 过滤案例

补充1：数组的内置方法`filter`,传匿名函数进去，循环数组元素传入匿名函数，如果返回true就保留该元素，如果返回false，就不保留

```javascript
// 补充：数组的内置方法，filter,传匿名函数进去，匿名函数如果返回true这个就保留，如果返回false，就不保留

var l = ['a', 'at', 'atom', 'atommon', 'be', 'beyond', 'cs', 'csrf', 'csrffe']
var ll = l.filter(function (item) {
    return 条件判断
})
console.log(ll)
```

补充2：字符串的`indexOf`方法，返回子字符串出现的索引位置，如果大于-1，表示子字符串在当前字符串中

```javascript
var s='lqz is handsome'
var res=s.indexOf('lqz') // 如果lzq在字符串中，就返回lzq在字符串的索引位置，即0
console.log(res)
```

将两个知识点结合起来，就可以实现筛选出包含搜索值的数组元素

```javascript
var search = 'a'
var l = ['a', 'at', 'atom', 'atommon', 'be', 'beyond', 'cs', 'csrf', 'csrffe']
var ll = l.filter(function (item) {
	return item.indexOf(search) > -1
})
console.log(ll)
```

补充3：this指向问题

```javascript
methods: {
        handleInput() {
            // this 指向问题
            // 这个位置this是vue对象
            var _this = this
            this.dataList = this.dataList.filter(function (item) {
                // 这个匿名函数内的this 还是vue对象吗？其实是浏览器对象，所以我们得定义一个_this，在匿名函数内使用_this而不是this
            	console.log('----', _this) 
            	return item.indexOf(_this.search) > -1
            })
    	}
}
```

补充4：箭头函数：没有自己的this，内部的this依然指向vue对象

```javascript
// 传统写法
var a=function (name,age){
    console.log(name)
}
a("lxx")

//es6 箭头函数  匿名函数的另一种写法
var a=  (name,age) =>{
    console.log(name)
}
```

因此用箭头函数就可以解决this指向的问题，简化代码

```javascript
// 使用es6的语法解决
this.newdataList = this.dataList.filter(item => {
    return item.indexOf(this.search) > -1
})
```

还有一个小问题，当我们对datalist做过滤时，datalist会产生永久性改变，下次input框内容变化后，也是在上一次过滤后的datalist的基础上做过滤，即datalist会越过滤越小，显示的值会越来越少，当输入框回退输入值时，筛选的值不会回退。

因此需要拷贝一个newdatalist，保持datalist不变，每次筛选都基于datalist生成newdatalist，然后循环newdatalist展示数据

案例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./js/vue.js"></script>

    <title>标题</title>

</head>
<body>


<div id="app">

    <h1>过滤案例</h1>
    <input type="text" v-model="search" @input="handleInput">
    <ul>
        <li v-for="item in newdataList">{{item}}</li>
    </ul>


</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            search: '',
            dataList: ['a', 'at', 'atom', 'atommon', 'be', 'beyond', 'cs', 'csrf', 'csrffe'],
            newdataList: ['a', 'at', 'atom', 'atommon', 'be', 'beyond', 'cs', 'csrf', 'csrffe']
        },
        methods: {
            handleInput() {
                // this 指向问题
                // 这个位置this是vue对象
                // var _this = this
                // this.dataList = this.dataList.filter(function (item) {
                //     console.log('----', _this) // this 还是vue对象吗？是浏览器对象
                //     return item.indexOf(_this.search) > -1
                // })

                // 使用es6的语法解决
                this.newdataList = this.dataList.filter(item => {
                    // console.log('----', this) // 如果使用剪头函数，this还是vue（剪头函数没有自己的this）
                    return item.indexOf(this.search) > -1
                })
            }
        }
    })


    // 补充：数组的内置方法，filter,传匿名函数进去，匿名函数如果返回true这个就保留，如果返回false，就不保留

    // var search = 'a'
    // var l = ['a', 'at', 'atom', 'atommon', 'be', 'beyond', 'cs', 'csrf', 'csrffe']
    // var ll = l.filter(function (item) {
    //     return item.indexOf(search) > -1
    // })
    // console.log(ll)

    // 补充：字符串有个indexOf，返回索引，如果大于-1，表示字字符串在当前字符串中
    // var s='lqz is handsome'
    // var res=s.indexOf('zoo') // 如果me在字符串中，就返回me在字符串的索引位置
    // console.log(res)


    // es6 的箭头函数
    // 传统写法
    // var a=function (name,age){
    //     console.log(name)
    // }
    // a("lxx")
    // //es6 箭头函数  匿名函数的另一种写法
    //  var a=  (name,age) =>{
    //     console.log(name)
    // }


</script>

</html>
```



### 事件修饰符

放在事件后的，修饰事件的，例如

`@click.once='函数'`修饰点击事件只能被触发一次

都有哪些常用的时间修饰符呢？

- .stop	只处理自己的事件，不再冒泡给父标签（阻止事件冒泡）
- .self	只处理自己的事件，子控件冒泡的事件不处理
- .prevent	阻止a链接的跳转
- .once	事件只会触发一次（适用于抽奖页面）

案例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./js/vue.js"></script>

    <title>标题</title>

</head>
<body>


<div id="app">

    <h1>点击子标签，父标签的事件也触发：事件冒泡</h1>
    <h2>阻止事件冒泡：点子标签，父标签不触发 stop</h2>
    <ul @click="handleUl">
        <li @click.stop="handleLi">点我看美女</li>
        <li>点我看帅哥</li>
    </ul>

    <h1>点击子标签，父标签的事件也触发：事件冒泡</h1>
    <h2>子标签的冒泡不处理：父标签写self，父标签只处理自己的事件，冒泡的事件不管</h2>
    <ul @click.self="handleUl">
        <li @click="handleLi">点我看美女</li>
        <li>点我看帅哥</li>
    </ul>

    <h1>prevent 阻止a链接的跳转</h1>
    <a href="http://www.liuqingzheng.top" @click.prevent="handleA">点我看美女</a>

    <h1>once 只执行一次</h1>
    <button @click.once="handleButton">点我秒杀</button>


</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {},
        methods: {
            handleLi() {
                console.log("li被点击了")
            },
            handleUl() {
                console.log("ul被点击了")
            },
            handleA() {
                console.log('a被点击了,不会再跳转了')
            },
            handleButton() {
                alert('秒杀成功')
            }
        }
    })

</script>

</html>
```

### 按键修饰符

输入框除了上面提到的常用事件外，还有一个按键被按下和被抬起触发的事件

- `keydown`：按键按下时触发
- `keyup`：按键释放时触发（常用）
- `keypress`：按下字符键时触发（不支持功能键）

可以使用`@keyup="函数"`绑定函数，再在函数中通过code判断被按下的按键

```javascript
handleKeyUp(event){
    if(event.code=='Enter') {
    	console.log('enter键被抬起了，开始搜索')
    }
}
```

同时对于常用的一些键，他给我们提供了对应的写法，如回车键

```html
<input type="text" @keyup.enter="函数">
```

这样就不需要在函数中判断被触发的按键是不是回车，因为只有被按下的是回车键才会触发该函数

函数中可以用`$event`来接收被按下的按键

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./js/vue.js"></script>

    <title>标题</title>

</head>
<body>


<div id="app">

    <h1>按键修饰符</h1>
    <!--    <input type="text" c>-->
    <input type="text" @keyup.enter="handleKeyUp($event)">
    <input type="text" @keyup.esc="handleKeyUp($event)">
    <input type="text" @keyup.="handleKeyUp($event)">


</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {},
        methods: {
            handleKeyUp(event) {
                console.log('sdasfd')
                if (event.code == 'Enter') {
                    console.log('enter键被抬起了，开始搜索')
                }
            }
        }

    })

</script>

</html>
```



## checkbox和radio的双向绑定

text类型的input标签实现数据绑定我们已经学过了，在表单中input标签还有checkbox和radio类型，要实现他们的双向数据绑定，下面我们来看看如何实现

checkbox的单选	用v-model绑定一个布尔值	只要选中布尔就为true，反之亦然

radio的单选	v-model绑定字符串	选中字符串就是value对应的值

checkbox的多选	v-model绑定一个数组	多个选中，数组就有多个值

这样实现表单数据的双向绑定后，只需要给登录按钮绑定一个点击事件，用ajax将数据传给后端即可

案例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <h1>表单控制之checkbox单选</h1>

        <p>用户名：<input type="text" v-model="username"></p>
        <p>密码：<input type="password" v-model="password"></p>
        <p><input type="checkbox" v-model="isCheck">记住密码</p>
        <p>
            <input type="radio" v-model="gender" value="1">男
            <input type="radio" v-model="gender" value="2">女
            <input type="radio" v-model="gender" value="0">未知
        </p>
        <p>爱好(多选)：</p>
        <p>
            <input type="checkbox" v-model="hobby" value="篮球"> 篮球
            <input type="checkbox" v-model="hobby" value="足球"> 足球
            <input type="checkbox" v-model="hobby" value="橄榄球"> 橄榄球
            <input type="checkbox" v-model="hobby" value="乒乓球"> 乒乓球
        </p>
<!--        <hr>-->
<!--        密码是否选中：{{isCheck}}-->
<!--        <hr>-->
<!--        性别是：{{gender}}-->
<!--        <hr>-->
<!--        爱好是：{{hobby}}-->

        <hr>
        <input type="submit" value="提交" @click="handleSubmit">



</div>

</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {
            username: '',
            password: '',
            isCheck: false,
            gender: '',
            hobby:[],
        },
        methods: {
            handleSubmit(){
                //假设发送ajax到后端了
                console.log(this.username)
                console.log(this.password)
                console.log(this.isCheck)
                console.log(this.gender)
                console.log(this.hobby)
            }
        },


    })
</script>
</html>
```



## 购物车案例

### js的循环方式

python，js，go，java  for循环的区别

```python
-python中for循环只有基于迭代的循环，没有基于索引的循环
    for xx in 可迭代对象  # 基于迭代
-js ：
  	for xx in 对象  # 基于迭代
    for i=0;i<10;i++  # 基于索引
-go：
	for i=0;i<10;i++  # 基于索引
    for item range 变量  # 基于迭代
      
-java：
    for i=0;i<10;i++  # 基于索引  1.8之前只有这种
    for xx in 对象  # 基于迭代   1.8后的语法
```

下面来看看js中常见的for循环

1. 最基本的

```javascript
for (let i=0; i<3; i++) {
  console.log(i)
}
```

2. in 循环  es5的语法

```javascript
for(let 成员 in 对象){
    循环的代码块
```

3. of   es6的循环

```javascript
for(item of arr){
    console.log('item =>', item)
  }
```

4. 数组foreach循环 (数组)

```javascript
var a=[33,22,888]
a.forEach(function (value,index){
	console.log(value)
    console.log(index)
})
```

5. jq  $each 循环

```javascript
$.each(可迭代对象,function (key,value) {
  });

// var a=[33,22,888]
var obj={name:'lqz',age:19}
 $.each(a,function (key,value){
       console.log(key)
       console.log(value)
   })
```

> 对于对象是key，value
>
> 对于数组是index，value



### 基本购物车

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
    <script src="./js/vue.js"></script>
    <script src="http://code.jquery.com/jquery-2.1.1.min.js"></script>
</head>
<body>
<div class="app">
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-6 col-md-offset-3">
                <div style="margin-top: 30px">
                    <h1>购物车案例</h1>
                    <table class="table table-bordered">
                        <thead>
                        <tr>
                            <th>商品id</th>
                            <th>商品名字</th>
                            <th>商品价格</th>
                            <th>商品数量</th>
                        </tr>
                        </thead>
                        <tbody>
                        <tr v-for="good in goodList">
                            <th>{{good.id}}</th>
                            <td>{{good.name}}</td>
                            <td>{{good.price}}</td>
                            <td>{{good.count}}</td>
                            <td><input type="checkbox" v-model="buyGoods" :value="good"></td>
                        </tr>
                        </tbody>
                    </table>
                    <hr>
                    选中的商品:{{buyGoods}}
                    <hr>
                    总价格是：{{getPrice()}}
                </div>


            </div>

        </div>

    </div>

</div>

</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {
            goodList: [
                {id: '1', name: '小汽车', price: 150000, count: 2},
                {id: '2', name: '鸡蛋', price: 2, count: 1},
                {id: '3', name: '饼干', price: 10, count: 6},
                {id: '4', name: '钢笔', price: 15, count: 5},
                {id: '5', name: '脸盆', price: 30, count: 3},
            ],
            buyGoods: [],
        },
        methods: {
            getPrice() {
                var total = 0
                // 方式一：数组的循环：循环计算选中的商品价格
                // this.buyGoods.forEach(function (v,i){
                //     total+=v.price*v.count
                // })

                // 方式二：es6 的 of 循环
                for (item of this.buyGoods) {
                    // console.log(item)
                    total += item.price * item.count
                }
                return total
            }
        }


    })

</script>
</html>
```



### 带全选，全不选

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-6 col-md-offset-3">
                <div style="margin-top: 30px">
                    <h1>购物车案例</h1>
                    <table class="table table-bordered">
                        <thead>
                        <tr>
                            <th>商品id</th>
                            <th>商品名字</th>
                            <th>商品价格</th>
                            <th>商品数量</th>
                            <th><input type="checkbox" v-model="checkAll" @change="handleCheckAll"></th>
                        </tr>
                        </thead>
                        <tbody>
                        <tr v-for="good in goodList">
                            <th>{{good.id}}</th>
                            <td>{{good.name}}</td>
                            <td>{{good.price}}</td>
                            <td>{{good.count}}</td>
                            <td><input type="checkbox" v-model="buyGoods" :value="good" @change="handleCheckOne"></td>
                        </tr>
                        </tbody>
                    </table>
                    <hr>
                    选中的商品:{{buyGoods}}
                    <hr>
                    总价格是：{{getPrice()}}
                </div>


            </div>

        </div>

    </div>

</div>

</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {
            goodList: [
                {id: '1', name: '小汽车', price: 150000, count: 2},
                {id: '2', name: '鸡蛋', price: 2, count: 1},
                {id: '3', name: '饼干', price: 10, count: 6},
                {id: '4', name: '钢笔', price: 15, count: 5},
                {id: '5', name: '脸盆', price: 30, count: 3},
            ],
            buyGoods: [],
            checkAll: false,
        },
        methods: {
            getPrice() {
                var total = 0
                for (item of this.buyGoods) {
                    // console.log(item)
                    total += item.price * item.count
                }
                return total
            },
            handleCheckAll() {
                if (this.checkAll) {
                    // 用户全选了，只需要把 buyGoods的值变为goodList
                    this.buyGoods = this.goodList
                } else {
                    this.buyGoods = []
                }
            },
            handleCheckOne() {
                // 判断buyGoods长度，是否等于goodList，如果等于说明用户全选了，把checkAll设置为true
                // 否则设置为false
                // if(this.buyGoods.length==this.goodList.length){
                //     // 说明用户通过单选，选全了
                //     this.checkAll=true
                // }else {
                //     this.checkAll=false
                // }
                // 简写成
                this.checkAll = (this.buyGoods.length == this.goodList.length)
            }
        }


    })

</script>
</html>
```

### 带加减

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <div class="container-fluid">
        <div class="row">
            <div class="col-md-6 col-md-offset-3">
                <div style="margin-top: 30px">
                    <h1>购物车案例</h1>
                    <table class="table table-bordered">
                        <thead>
                        <tr>
                            <th>商品id</th>
                            <th>商品名字</th>
                            <th>商品价格</th>
                            <th>商品数量</th>
                            <th><input type="checkbox" v-model="checkAll" @change="handleCheckAll"></th>
                        </tr>
                        </thead>
                        <tbody>
                        <tr v-for="good in goodList">
                            <th>{{good.id}}</th>
                            <td>{{good.name}}</td>
                            <td>{{good.price}}</td>
                            <td>
                                <button @click="handleJian(good)">-</button>
                                {{good.count}}
                                <button @click="good.count++">+</button>
                            </td>
                            <td><input type="checkbox" v-model="buyGoods" :value="good" @change="handleCheckOne"></td>
                        </tr>
                        </tbody>
                    </table>
                    <hr>
                    选中的商品:{{buyGoods}}
                    <hr>
                    总价格是：{{getPrice()}}
                </div>


            </div>

        </div>

    </div>

</div>

</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {
            goodList: [
                {id: '1', name: '小汽车', price: 150000, count: 2},
                {id: '2', name: '鸡蛋', price: 2, count: 1},
                {id: '3', name: '饼干', price: 10, count: 6},
                {id: '4', name: '钢笔', price: 15, count: 5},
                {id: '5', name: '脸盆', price: 30, count: 3},
            ],
            buyGoods: [],
            checkAll: false,
        },
        methods: {
            getPrice() {
                var total = 0
                for (item of this.buyGoods) {
                    // console.log(item)
                    total += item.price * item.count
                }
                return total
            },
            handleCheckAll() {
                if (this.checkAll) {
                    // 用户全选了，只需要把 buyGoods的值变为goodList
                    this.buyGoods = this.goodList
                } else {
                    this.buyGoods = []
                }
            },
            handleCheckOne() {
                // 判断buyGoods长度，是否等于goodList，如果等于说明用户全选了，把checkAll设置为true
                // 否则设置为false
                // if(this.buyGoods.length==this.goodList.length){
                //     // 说明用户通过单选，选全了
                //     this.checkAll=true
                // }else {
                //     this.checkAll=false
                // }
                // 简写成
                this.checkAll = (this.buyGoods.length == this.goodList.length)
            },
            handleJian(item) {
                if (item.count <= 1) {
                    alert('太少了，受不了了')
                } else {
                    item.count--
                }
            }
        }


    })

</script>
</html>
```



## v-model进阶

我们来学习几个v-model的进阶操作

lazy：双向数据绑定，只要修改输入框的值，就更新数据，耗费资源，加了lazy后，等输入动作结束后，变量再变化

number：输入框，只接收数字部分，如果输入了  123asdfasd，只保留123 数字部分。但如果输入的是adbawjd112，就不会做任何操作

trim：  去掉输入内容前后的空白

语法：`v-model.lazy='变量'`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <h1>v-model进阶</h1>
    <p><input type="text" v-model.lazy="a">{{a}}</p>
    <p><input type="text" v-model.number="b">{{b}}</p>
    <p><input type="text" v-model.trim="c">{{c}}</p>


</div>

</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {
            a: '',
            b: '',
            c: '',

        },


    })
</script>
</html>
```

## Vue生命周期

vue的实例：vm，以及后面学的组件：vc 它们有生命周期

从创建开始，到销毁，总共有8个钩子函数(4对)，依次调用

什么是钩子函数？

主流编程思想

```
面向过程编程
OOP编程：面向对象编程
AOP编程：面向切面编程（对面向对象编程的补充），java的spring框架大量的使用
	-python中装饰器  AOP编程的体现
	-drf源码，反序列化的源码，钩子函数，aop体现
```

而钩子就体现了aop编程的思想

下面我们来介绍Vue的八个钩子函数

```javascript
beforeCreate	创建Vue实例，组件实例对象创建 之前调用
created	        创建Vue实例成功后调用（常用，可以在此处发送ajax请求后端数据）

beforeMount	    渲染DOM之前调用
mounted	        渲染DOM之后调用
    ---初始化完成了----

beforeUpdate	重新渲染之前调用（数据更新等操作时，控制DOM重新渲染）
updated	         重新渲染完成之后调用

    ---一直循环调用这两个钩子----

    ----销毁组件---
beforeDestroy	 销毁之前调用
destroyed	     销毁之后调用

```

案例：

我们选择用观看组件的生命周期，因为vm实例的生命周期，在销毁时页面已经关闭，为了完整的看到Vue的生命周期，我们选择组件，它可以自由的创建和销毁

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">


    <button @click="handleShow">点我，组件显示和消失</button>
    <hr>
    <lqz v-if="show"></lqz>
    <hr>


</div>

</body>
<script>

    // 定义一个组件  `` es6语法的模版字符串  `lqz is ${变量}`
    Vue.component('lqz', {
            template: `
              <div>
                <h1>我是一个好看的组件{{ name }}</h1>
                <button @click="handleClick">点我看美女</button>
              </div>`,
            data() {
                return {
                    name: '彭于晏',
                    t: '',
                }
            },
            methods: {
                handleClick() {
                    this.name = 'lqz'
                }
            },
            beforeCreate() {
                console.log('beforeCreate')
                console.log(this.$data) // 数据部分
                console.log(this.$el) // 模版
            },
            created() {
                console.log('created')
                console.log(this.$data) // 数据部分
                console.log(this.$el) // 模版
                // 启动一个定时器（实时前后端通信，web微信，开启了定时器一直在向后端发请求拿数据）
                this.t = setInterval(function () {
                    console.log('hello world')
                }, 3000)   // 过一段时间就执行，一直执行
                // setTimeout() //延迟调用，执行一次
            },
            beforeMount() {
                console.log('beforeMount')
                console.log('当前el状态：', this.$el)
                console.log('当前data状态：', this.$data)
            },
            mounted() {
                console.log('mounted')
                console.log('当前el状态：', this.$el)
                console.log('当前data状态：', this.$data)

            },
            beforeUpdate() {
                console.log('beforeUpdate')
                console.log('当前el状态：', this.$el)
                console.log('当前data状态：', this.$data)
            },
            updated() {
                console.log('updated')
                console.log('当前el状态：', this.$el)
                console.log('当前data状态：', this.$data)
            },
            beforeDestroy() {
                console.log('beforeDestroy')
                console.log('当前el状态：', this.$el)
                console.log('当前data状态：', this.$data)
            },
            destroyed() {
                console.log('destroyed')
                console.log('当前el状态：', this.$el)
                console.log('当前data状态：', this.$data)
                // 释放掉定时器
                clearInterval(this.t)
                this.t = null


            },
        }
    )


    var vm = new Vue({
        el: '.app',
        data: {
            show: true
        },
        methods: {
            handleShow() {
                this.show = !this.show
            }
        },

    })
</script>
</html>
```

总结：

比较常用的几个钩子

- created：此时data的数据完成了初始化，在这里发送ajax请求去后端请求数据
- updated：每次数据变化，页面更新完后，都会执行它
- destroyed：组件销毁，会触发它，用于完成资源清理工作，例如关闭组件中启用的定时器等等

不管是vm实例还是组件实例，都有8个生命周期钩子函数，写了钩子函数，就会执行，不写就不会执行



## 与后端交互(ajax,fetch和axios)

跨域问题： 前后端分离的项目会出现

- 浏览器有个安全策略：不允许向不同域(地址+端口号)发送请求获取数据，这个安全策略叫做浏览器的同源策略

如何解决跨域问题？

- 后端的cors(跨域资源共享)技术：就是在响应头中加入允许即可

- nginx做代理



前后端数据交互主要有三种方式

原生js发送ajax请求，jq发送ajax请求，fetch发送ajax请求，axios发送ajax请求

- 原生：new XMLHttpRequest()   老api，坑很多

- jq基于它封装，封装出了$.ajax ,屏蔽了很多问题
- 官方觉得XMLHttpRequest坑很多，搞了个fetch，跟XMLHttpRequest是平级的，比它好用，但是不支持ie
- axios继续基于XMLHttpRequest封装了一个发送ajax请求的模块

原生的和fetch是官方提供的，不需要导入，而jq和axios是第三方的框架或者模块，需要导入

### 方式一：使用jq的ajax方法

原生js写ajax，jq帮咱们写好了，处理好了浏览器版本间的不兼容，可以直接用

```python
$.ajax({
	url: 'http://127.0.0.1:8000/test/',
    type: 'get',
    success: data => {
    // 后端返回json格式字符串，$.ajax拿到字符串，转成了js的对象，给了data
    	this.name = data.name
        this.age = data.age
    }
})
```

### 方式二：使用fetch

解释一个关于es6箭头函数的小知识

如果函数体内只有一行代码，可以省略return和{}

```javascript
var a = function (data) {
    return data + 'lqz'
}

var a = data => data + 'lqz'
```



```python
fetch('http://127.0.0.1:8000/test/').then(res => res.json()).then(res => {
    console.log(res)
    this.name = res.name 
    this.age = res.age
})
```

### 方式三：使用第三方，axios(js模块，需要导入)

```python
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
 
  
axios.get('http://127.0.0.1:8000/test/').then(res=>{
    console.log(res.data) // 真正后端给的数据封装在res.data中
    this.name = res.data.name
    this.age=res.data.age
 })
```



### 从后端加载数据，显示电影的小案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
</head>
<body>
<div class="app">

    <div>
        <ul>
            <li v-for="film in filmsList">
                <h3>电影标题：{{film.name}}</h3>
                <img :src="film.poster" alt="" width="200px" height="400px">
                <p>简介：{{film.synopsis}}</p>
            </li>
        </ul>
    </div>

</div>

</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {
            filmsList: []
        },
        methods: {},
        created() {
            axios.get('http://127.0.0.1:8000/films/').then(res => {
                this.filmsList = res.data.data.films
            })
        }


    })
</script>
</html>
```



## 计算属性

```python
computed: {
    函数名(){
		return XXX
	}
}
```

将函数写在computed中

特点：

- 可以将函数当属性使用

- 只有关联的数据发生变化，才会重新执行函数运算结果

- 支持缓存，当数据未发生变化时，页面更新，他也不会变化

案例：有个input，输入英文后，把首字母变成大写

如果不采用计算属性，而是选择直接在插值语法中插入表达式或者函数，当页面其他部分发生变化，该数据也会重新计算，比较耗费资源

采用计算属性，就相当于将函数当变量用，只有该函数相关的变量发生变化，才会重新计算函数，修改变量。切记该函数一定要return值

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <h1>计算属性</h1>
    <!--    最简单方式-->
    <h2>最简单方法首字母变大写</h2>
    <!--    <input type="text" v-model="name">-&ndash;&gt;{{name.slice(0, 1).toUpperCase() + name.slice(1)}}-->
    <h2>使用函数实现，页面只要更新，这个函数就会重新执行,比较耗费资源</h2>
    <!--    <input type="text" v-model="name">-&ndash;&gt;{{getUpper()}}-->

    <input type="text" v-model="age">--->{{age}}

    <h3>通过计算属性实现：计算属性只有在它的相关依赖发生改变时才会重新求值</h3>
    <input type="text" v-model="name">--->{{upperName}}

</div>

</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {
            name: '',
            age: 0
        },
        methods: {
            getUpper() {
                console.log('我执行了')
                return this.name.slice(0, 1).toUpperCase() + this.name.slice(1)
            }
        },
        computed: {
            upperName() {
                console.log('我是计算属性，我执行了')
                return this.name.slice(0, 1).toUpperCase() + this.name.slice(1)
            }
        }


    })
</script>
</html>
```

案例2：通过计算属性重写过滤案例

只要input框中search数据发生变化，newdataList会重新运算，只要一运算，v-for重新循环，就会显示新的过滤结果

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./js/vue.js"></script>

    <title>标题</title>

</head>
<body>


<div id="app">

    <h1>过滤案例</h1>
    <input type="text" v-model="search">
    <ul>
        <li v-for="item in newdataList">{{item}}</li>
    </ul>


</div>
</body>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            search: '',
            dataList: ['a', 'at', 'atom', 'atommon', 'be', 'beyond', 'cs', 'csrf', 'csrffe'],

        },
        // 只要input框中数据发生变化search，newdataList会重新运算，只要一运算，v-for重新循环，实现了联动
        computed: {
            newdataList() {
                return this.dataList.filter(item => {
                    return item.indexOf(this.search) > -1
                })
            }
        }
    })


</script>

</html>
```



## 监听属性

监听categoryType变量，只要发生变化，执行对应的函数

```python
watch: {
    categoryType: function (val) {
        console.log('name发生了变化')
        console.log(val)
    }
}
```

应用场景：前端有分类标签按钮，点击向后端发送请求，请求回相应分类的数据，展示

如果不使用监听属性，需要给每一个标签绑定一个点击事件，触发函数去后端请求对应数据

而使用监听属性，只需要在点击事件中对被监听变量进行赋值，就会自动触发函数发起请求

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <h1>监听属性</h1>
    <span @click="categoryType='美女'">美女</span>|
    <span @click="categoryType='帅哥'">帅哥</span>|
    <span @click="categoryType='老头'">老头</span>


    <div>

    </div>
</div>

</body>
<script>
    var vm = new Vue({
        el: '.app',
        data: {
            categoryType: '',
        },
        watch: {
            categoryType: function (val) {
                console.log('name发生了变化')
                console.log(val)
                // 向后端发送请求，请求回相应分类的数据，展示
            }
        }


    })
</script>
</html>
```



## 组件化开发基础

作用：扩展 HTML 元素，封装可重用的代码，目的是复用

- 例如：有一个轮播，可以在很多页面中使用，一个轮播有js，css，html

- 组件把js，css，html放到一起，有逻辑，有样式，有html

多组件页面和单组件页面

- 官方推荐，以后一个组件是一个 xx.vue 文件 ---》webpack编译

Single-Page application，缩写为 SPA：以后vue项目只有一个页面，看到的页面变化都是组件间的切换



### 全局组件

```javascript
Vue.component('navbar', obj)
obj = {
    template: ``,
    data() {
        return {}
    },
    methods: {}
}
```



在任意组件中都可以使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <h1>全局组件</h1>
    <navbar></navbar>


</div>

</body>
<script>
    // 定义一个全局组件，在任意组件中都可以使用
    var obj = {
        template: `
          <div>
            <button @click="handleBack">后退</button>
            我是一个组件-->{{ name }}
            <button>前进</button>
          </div>
        `,
        data() {
            return {
                name: 'lqz'
            }
        }, // 重点，data必须是个函数，返回一个对象，组件可以在多个地方重复使用，如果就是对象，导致多个组件的数据指向同一块内存地址，引起错乱
        methods: {
            handleBack() {
                alert('后退了')
            }
        },
        // 学的所有放在vm对象中的，都可以用
    }
    Vue.component('navbar', obj)


    var vm = new Vue({
        el: '.app',
        data: {},


    })
</script>
</html>
```



### 局部组件-->只能在某个组件中使用



```javascript
components: {
    组件名: {
        template: ``,
        data() {
            return {}
        },
        methods: {}
     }
}
```

如下案例中，局部组件只能在vm内使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <h1>全局组件</h1>
    <navbar></navbar>
    <hr>
    <lqz></lqz>


</div>

</body>
<script>
    // 定义一个全局组件，在任意组件中都可以使用
    var obj = {
        template: `
          <div>

            <button @click="handleBack">后退</button>
            我是一个组件-->{{ name }}
            <button>前进</button>
          </div>
        `,
        data() {
            return {
                name: 'lqz'
            }
        }, // 重点，data必须是个函数，返回一个对象，组件可以在多个地方重复使用，如果就是对象，导致多个组件共用同一个对象的数据，出现错乱
        methods: {
            handleBack() {
                alert('后退了')
            }
        },
        // 学的所有放在vm对象中的，都可以用
        components: {} // 全局组件下又可以定义局部组件，该组件可以在全局组件内部调用
    }
    Vue.component('navbar', obj)


    var vm = new Vue({
        el: '.app',
        data: {},
        components: {
            lqz: {
                template: `
                  <div>
                    <h3>我是局部组件</h3>
                    <button>点我看美女</button>
                  </div>`,
                data() {
                    return {}
                },
                methods: {}
            }
        }


    })
</script>
</html>
```



## 组件通信

各个组件间，数据，方法，都是隔离的，要实现跨组件使用数据，就要实现组件通信

### 父子通信之父传子（自定义属性）

在子组件中自定义属性接收父组件的变量，然后在子组件的props列表中声明属性名，就可以在子组件中使用父组件的值了

```html
<navbar :myname="name" :age="age"></navbar>

props: ['myname', 'age']
```



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <h1>通过自定义属性，实现父传子</h1>
    <h3>父组件中name:{{name}}</h3>
    <hr>
    <navbar :myname="name" :age="age"></navbar>
    <hr>

</div>

</body>
<script>
    // 定义一个全局组件，在任意组件中都可以使用

    Vue.component('navbar', {
        template: `
          <div>
            <button>前进</button>
            名字是：{{ myname }}--->年龄是：{{ age }}
            <button>后退</button>
          </div>`,
        props: ['myname', 'age']
    })


    var vm = new Vue({
        el: '.app',
        data: {
            name: '彭于晏',
            age: 99
        },


    })
</script>
</html>
```



### 父子通信之子传父(自定义事件)

- 点击按钮触发子组件事件handleClick
- 子组件执行函数this.$emit('自定义事件名字')，并将子组件的变量传递给自定义事件myevent
- 注册在子组件上的（在父组件内）自定义事件对应的函数handleReceive执行，即可将传递过来的子组件的变量赋值父组件的变量

本质：子组件把自己的值传出来，拿到父组件内，父组件内的函数实现赋值操作，将变量永久存储在父组件的变量中

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <h1>通过自定义事件，实现子传父</h1>
    <h3>父组件中收到子组件传递对值为:{{name}}</h3>
    <hr>
    <navbar @myevent="handleReceive"></navbar>
    <hr>

</div>

</body>
<script>
    Vue.component('navbar', {
        template: `
          <div>
            <input type="text" v-model="iname">---》{{ iname }}
            <br>
            <button @click="handleClick">点我吧iname传递到父组件中</button>
          </div>`,
        data() {
            return {
                iname: ''
            }
        },
        methods: {
            handleClick() {
                // 触发自定义事件的执行，并且传入当前组件中的iname
                this.$emit('myevent', this.iname)
                // alert(this.iname)
            }
        }
    })

    var vm = new Vue({
        el: '.app',
        data: {
            name: '',
        },
        methods: {
            handleReceive(iname) {
                this.name = iname
            }
        }


    })

    //子组件点击按钮=>子组件执行函数this.$emit('自定义事件名字') =》注册在组件上的【自定义事件】对应的函数执行
</script>
</html>
```



### ref属性

对于组件间通信，vue提供了一个ref属性，可以放在任意标签

- 放在普通标签,通过  this.$refs.ref对应的名字    就能拿到原生dom对象,使用原生操作该dom
- 放在自定义组件上,通过 this.$refs.ref对应的名字 就能拿到 组件对象，就可以调用对象的函数，使用对象的变量
- 父组件中，拿到了子组件对象，对象中的属性，方法可以直接用，直接改

通过ref，子传父

- 因为在父中，可以拿到子的对象，子对象中的所有变量，方法，都可以直接用

通过ref，实现父传子

- 因为在父中，可以拿到子的对象， 子对象.变量=父的变量

ref实现的还是父子之间的通信，要实现跨组件的通信，需要用到vuex



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <h1>ref实现组件间通信</h1>
    	{{name}}
    <br>
    	<input type="text" v-model="name" ref="myinput">
    	<button @click="handleClick">点我打印</button>
    <hr>
    	<navbar ref="mynavbar"></navbar>
    <hr>
    	<button @click="handleClick1">点我</button>


</div>

</body>
<script>
    Vue.component('navbar', {
        template: `
          <div>
          <input type="text" v-model="iname">---》{{ iname }}
          <br>
          </div>`,
        data() {
            return {
                iname: ''
            }
        },
        methods: {
            handleClick() {
                console.log('执行了')
                return 'xxx'
            }
        }
    })
    var vm = new Vue({
        el: '.app',
        data: {
            name: '',
        },
        methods: {
            handleClick() {
                console.log(this.$refs.myinput)
                console.log(this.$refs.myinput.value)
                this.$refs.myinput.value = 'lqz is big'
                alert(this.name)
            },
            handleClick1(){
                console.log(this.$refs.mynavbar) // 相当于拿到了再组件中的this（组件对象）
                console.log(this.$refs.mynavbar.iname)
                // this.name=this.$refs.mynavbar.iname
                // 父组件中直接执行子组件的方法
                // this.$refs.mynavbar.handleClick() // 调用子组件的方法
                this.$refs.mynavbar.iname='sssss'


            }
        }


    })

</script>
</html>
```



## 动态组件

`<component>`：动态地绑定多个组件到它的 is 属性，is属性等于哪个组件的name，就显示哪个组件

场景： 组件是谁不确定，通过 is 确定显示那个组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <h1>动态组件</h1>
    <ul>
        <li @click="myType='home'">首页</li>
        <li @click="myType='goods'">商品</li>
        <li @click="myType='shopping'">购物</li>
    </ul>
    <!--    动态组件，is是哪个组件名字，这里就会显示那个组件-->
    <component :is="myType"></component>


</div>

</body>
<script>

    Vue.component('home', {
        template: `
            <div>
                <h2>首页</h2>
            </div>`
    })

    Vue.component('goods', {
        template: `
            <div>
                <h2>商品</h2>
            </div>`
    })

    Vue.component('shopping', {
        template: `
            <div>
                <h2>购物</h2>
            </div>`
    })


    var vm = new Vue({
        el: '.app',
        data: {
            myType: 'home',
        },


    })

</script>
</html>
```

`<keep-alive>` 保留状态，避免重新渲染



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <h1>动态组件</h1>
    <ul>
        <li @click="myType='home'">首页</li>
        <li @click="myType='goods'">商品</li>
        <li @click="myType='shopping'">购物</li>
    </ul>
    <!--    动态组件，is是哪个组件名字，这里就会显示那个组件-->
    <keep-alive>
        <component :is="myType"></component>

    </keep-alive>
</div>

</body>
<script>

    Vue.component('home', {
        template: `
            <div>
                <h2>首页</h2>
            </div>`
    })

    Vue.component('goods', {
        template: `
            <div>
                <h2>商品</h2>
                要搜索的商品：<input type="text">
            </div>`
    })

    Vue.component('shopping', {
        template: `
            <div>
                <h2>购物</h2>
            </div>`
    })


    var vm = new Vue({
        el: '.app',
        data: {
            myType: 'home',
        },


    })
</script>
</html>
```



## slot插槽

### 不具名插槽

在组件定义时预留一个slot标签，然后在使用时写在组件内的标签都会被插入到slot标签中

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <h1>插槽</h1>
    <hr>
    <home>
        <div>
            <img src="https://pic.maizuo.com/usr/movie/0c5874a81c7969c8eb7ddee7b6ff46b3.jpg" alt="" width="200px"
                 height="300px">
        </div>
    </home>
    <hr>

    <home>
        <div>
            我是div
        </div>
    </home>

</div>

</body>
<script>

    Vue.component('home', {
        template: `
            <div>
                <h2>首页</h2>
                <slot></slot>
            </div>`
    })

    var vm = new Vue({
        el: '.app',
        data: {
            myType: 'home',
        },
    })

</script>
</html>
```

### 具名插槽

在组件定义时为slot标签指定name

使用插槽时指定slot属性值为对应的name，即可精准插入

```html
<!--使用插槽-->
<home>
    <img src="https://pic.maizuo.com/usr/movie/0c5874a81c7969c8eb7ddee7b6ff46b3.jpg" alt="" width="200px"
         height="300px" slot="bottom">
    <div slot="top">
         我是div
    </div>
</home>


<!--组件定义-->
Vue.component('home', {
        template: `
            <div>
                <slot name="top"></slot>
                <p>我是子组件</p>
                <slot name="bottom"></slot>

            </div>`
    })
```



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/vue.js"></script>
</head>
<body>
<div class="app">
    <h1>插槽</h1>
    <hr>
    <home>
        <img src="https://pic.maizuo.com/usr/movie/0c5874a81c7969c8eb7ddee7b6ff46b3.jpg" alt="" width="200px"
             height="300px" slot="bottom">
        <div slot="top">
            我是div
        </div>
    </home>
    <hr>


</div>

</body>
<script>

    Vue.component('home', {
        template: `
            <div>
                <slot name="top"></slot>
                <p>我是子组件</p>
                <slot name="bottom"></slot>

            </div>`
    })

    var vm = new Vue({
        el: '.app',
        data: {
            myType: 'home',
        },


    })


</script>
</html>
```



## Vue-CLI 项目搭建

写一个一个独立的页面比较麻烦，这里就要引出一个前端工程化的概念，比如我们的python框架，开发后端接口就是一个独立的项目，那么能不能把前端开发也当成是一个项目呢？
vue是一个前端项目，需要webpack支持，同时vue官方提供了一个工具：vue-cli

vue-cli：vue的脚手架，快速创建一个vue项目，带了很多文件

> vue2 中，都是使用vue-cli
>
> vue3中，可以使用vue-cli创建，官方更推荐使用 vite ，更块，更小

Vue-CLI 是基于nodejs的

- nodejs：是一门后端编程语言，js语法，运行在操作系统上，后端语言，跟python一样，是一个解释型语言，需要安装解释器

搭建node：下载自己平台对应的版本即可，添加环境变量

- node：相当于python
- npm：包管理工具，相当于pip



安装 vue-cli：

- 配置cnpm（淘宝定制，去淘宝镜像站下载，比npm下载更快）

  - ```
    npm install -g cnpm --registry=https://registry.npmmirror.com
    ```

  - 这样以后就可以用cnpm安装

  - 如果要永久替换，执行下述命令

  - ```
    npm config set registry https://registry.npmmirror.com
    ```

  - 永久替换后，使用npm指令就默认是去淘宝的镜像站下载

- 安装vue-cli

  - ```
    cnpm install -g @vue/cli
    ```



创建项目：

使用vue脚手架，创建vue项目，装完脚手架，就会有个  vue  命令

- vue create 项目名
  - 命令行创建
- vue ui
  - 图形化界面创建

命令行创建说明：

![image-20250201161451705](/post/Vue/Vue2/image-20250201161451705.png)

第三方组件选择

![image-20250201161024347](/post/Vue/Vue2/image-20250201161024347.png)

vue版本

![image-20250201161110040](/post/Vue/Vue2/image-20250201161110040.png)

包管理文件

![image-20250201161539702](/post/Vue/Vue2/image-20250201161539702.png)

创建成功后就可以使用pycharm打开项目了

![image-20250201161904849](/post/Vue/Vue2/image-20250201161904849.png)



### vue项目启动

- 方式一：终端执行`npm run serve`

- 方式二：配置pycharm的启动按钮

![image-20250201162632956](/post/Vue/Vue2/image-20250201162632956.png)



### vue项目目录文件介绍

```python
vue2_test        # 项目名
  node_modules   # 类似于学的python的虚拟环境，主要放了当前项目所有的依赖，很多小文件，给别人发送项目需要把它删掉，执行命令npm install即可重新下载
  public         # 文件夹
    favicon.ico  # 网站小图标
    index.html   # 单页面应用(spa) <div id="app"></div>
  src            # 重点：文件夹，以后咱们的代码，都写在这里，js，组件，启动文件
    assets       # 文件夹，放静态资源，图片，js，css
    	logo.png   # 图片
    components   # 文件夹，内部放所有的小组件，非页面组件
    	HelloWorld.vue # 写了一个默认的HelloWorld的组件，是以 .vue结尾都
    router       # 文件夹，vue-router模块安装了就会有，主要做路由配置
    	index.js   # vue-router的js文件
    store        #  文件夹，vuex模块安装了就会有，主要做vuex的配置
    	index.js   # vuex的js文件
    views        # 文件夹，放组件，放页面组件
    	AboutView.vue # 关于的页面组件
    	HomeView.vue  # 首页的页面组件
    App.vue      # 根组件 vm对象就是它，它管理了index.html的id为app的div
    main.js      # 整个入口，非常重要，很多全局配置
  .gitignore     # git忽略文件相关
  babel.config.js# babel的配置，咱们不用管
  jsconfig.json  # 不需要关注
  package.json   # 重要：项目依赖的所有第三方模块 等同于python中  requirements.txt
  							 # 执行了 npm install -S axios,这里面就会多这一条
  package-lock.json # 锁定版本
  README.md      # 项目介绍
  vue.config.js  # vue的配置文件  等同于 django中setting.py
```

![image-20250204164419225](/post/Vue/Vue2/image-20250204164419225.png)

### vue项目开发技巧

项目入口

`src/main.js`

```javascript
// 模块的导入
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

// 修改vue的提示信息，关闭开发模式
Vue.config.productionTip = false

// 不用动
new Vue({
    router,
    store,
    render: h => h(App)  // 等同于data，method等等
}).$mount('#app')  // 等同于el，与index.html中的div标签绑定

```

以后组件化开发，就是写一个个的组件，页面组件，小组件，每一个组件都是xx.vue：

App.vue：根组件

如何开发一个组件？

第一部分：

```vue
<template>
  <div id="app"> # 组件必须套在一个标签内部，里面写上之前用反引号写的html内容
  </div>
</template>
```

第二部分：

```vue
<script>
export default {
// 以后在这个对象中写咱们之前学的data，methods，watch，computed，生命周期钩子
  name: 'App',
  data() {
    return {
      name: 'lqz'
    }
  }
  ,
  methods: {
    handleClick() {
      this.name = '彭于晏'
    }

  }
}
</script>
```

第三部分：

```vue
<style>
h1 {
  background-color: red;
}
</style>
```

如何在根组件中使用自定义的组件？
例如，我们创建了一个名为HelloWorld.vue的组件

自定义组件`src/components/HelloWorld.vue`

```vue
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    x的值是：{{ x }},y的值是：{{ y }}
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: ['msg'],
//父传子，自定义属性，接受
  data() {
    return {
      x: 10,
      y: 20
    }
  }
}
</script>
```

根组件`src/App.vue`

先导入自定义组件，再在components中注册为局部组件，在template中使用

```vue
<template>
  <div id="app">
    <h1>我是h1</h1>
    {{ name }}
    <br>
    <button @click="handleClick">点我名字变化</button>
    <hr>
    <HelloWorld :msg="msg"></HelloWorld>
    <!--    自定义属性实现父传子-->
  </div>
</template>

<style>
h1 {
  background-color: red;
}
</style>

<script>
// 把HelloWorld组件导入
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: 'App',
  data() {
    return {
      name: 'lqz',
      msg: '你好啊'
    }
  },
  methods: {
    handleClick() {
      this.name = '彭于晏'
    }

  },
  components: {
// 注册局部组件
    HelloWorld
  }
}
</script>
```



## es6导入导出语法

写vue项目，大量的看到：他们是es6的导入和导出语法，比如

```javascript
import App from './App.vue'
export default{}
```

- 1、可以在任意位置写xx.js,可以定义变量，定义函数

- 2、导出某些变量、函数

  - 2.1 默认导出

    - ```javascript
      export default {
           name,print
      }
      ```

  - 2.2 命名导出

    - ```javascript
      export const a =10
      ```

- 3、在想用的地方导入

  - ```javascript
    import 别名  form '路径'
    ```

- 4、使用

  - ```javascript
    别名.name
    别名.print()
    ```

案例：在main.js中使用自己编写的test.js文件中的变量以及函数

`src/assets/js/test.js`

```javascript
var name = '刘亦菲'
var age = 99

function print() {
    console.log('hello world')
    console.log(age)
}

// 默认导出，才能在别的地方导入
export default {name, print}
// export default {name: name, print: print}

```

`src/main.js`

```javascript
// 模块的导入
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

// 修改vue的提示信息，关闭开发模式
Vue.config.productionTip = false

//在这里使用test.js中定义的变量和函数，再导入之前，必须在test.js中导出才行
import test from './assets/js/test.js'
//使用print函数
test.print()
console.log(test.name)
//无法使用未导出的变量
console.log(test.age)


// 不用动
new Vue({
    router,
    store,
    render: h => h(App)  // 等同于data，method等等
}).$mount('#app')  // 等同于el，与index.html中的div标签绑定

```

补充：如果一个包下有一个名为index.js 的文件，可以只导入到包这一层，index.js 可以省略

```javascript
import lqz from './lqz/index'
# 简写
import lqz from './lqz'
```

学了es6的导入导出语法后，再回头理解一下main.js

```javascript
// 模块的导入
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

// 修改vue的提示信息，关闭开发模式
Vue.config.productionTip = false

// 不用动
new Vue({
    router,
    store,
    render: h => h(App)  // 等同于data，method等等
}).$mount('#app')  // 等同于el，与index.html中的div标签绑定

```

App.vue

```javascript
<template>
  <div id="app">
    <h1>我是h1</h1>
    {{ name }}
    <br>
    <button @click="handleClick">点我名字变化</button>
    <hr>
    <HelloWorld :msg="msg"></HelloWorld>
    <!--    自定义属性实现父传子-->
  </div>
</template>

<style>
h1 {
  background-color: red;
}
</style>

<script>
// 把HelloWorld组件导入
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: 'App',
  data() {
    return {
      name: 'lqz',
      msg: '你好啊'
    }
  },
  methods: {
    handleClick() {
      this.name = '彭于晏'
    }

  },
  components: {
// 注册局部组件
    HelloWorld
  }
}
</script>
```

在App.vue中默认导出了以下内容。封装为一个对象，在main.js中命名为App

```javascript
{
  name: 'App',
  data() {
    return {
      name: 'lqz',
      msg: '你好啊'
    }
  },
  methods: {
    handleClick() {
      this.name = '彭于晏'
    }

  },
  components: {
// 注册局部组件
    HelloWorld
  }
}
```

所以App就是App.vue定义的组件对象，同样的，在App.vue中引入HelloWorld组件

```javascript
// HelloWorld.vue
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    x的值是：{{ x }},y的值是：{{ y }}
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: ['msg'],
//父传子，自定义属性，接受
  data() {
    return {
      x: 10,
      y: 20
    }
  }
}
</script>
```



```javascript
// App.vue
components: {
	// 注册局部组件
    // HelloWorld:HelloWorld
    HelloWorld
}
```

等同于

```javascript
components: {
	HelloWorld:{
  		name: 'HelloWorld',
  		props: ['msg'],
		//父传子，自定义属性，接受
  		data() {
    		return {
      		x: 10,
     		y: 20
    		}
  		}
	}
}
```





## axios与后端交互处理跨域，携带数据，携带请求头

### 使用axios与后端交互

安装axios

```vue
npm install -S axios
```

使用

```javascript
import axios from 'axios'

axios.get('http://127.0.0.1:8000/user/').then(res=>{
    console.log(res)
    this.name=res.data.name
    this.age=res.data.age
})
```

后端需要添加响应头`'Access - Control - Allow - Origin':'*'`

解决跨域问题（浏览器的同源策略）

- 后端使用cors技术
- 使用nginx转发
- 前端做代理：只在测试阶段使用，上线后没有



### 处理跨域

在开发时，常用后端使用cors技术解决跨域问题，需要第三方库django-cors-headers

安装

- ```python
  pip install django-cors-headers
  ```

使用

- 注册app

  - ```python
    INSTALLED_APPS = (
    	...
    	'corsheaders',
    	...
    )
    ```

- 添加中间件

  - ```python
    MIDDLEWARE = [ 
    	...
    	'corsheaders.middleware.CorsMiddleware',
    	...
    ]
    ```

  - 需要注意的是，它应该放在其他可能会生成响应的中间件之前，比如`django.middleware.common.CommonMiddleware`，这样才能正确地设置响应头

- 其他配置

  - ```python
    CORS_ALLOW_CREDENTIALS = True
    # 允许所有的源进行跨域访问
    CORS_ORIGIN_ALLOW_ALL = True
    # 允许跨域访问的源列表
    CORS_ORIGIN_WHITELIST = (
    	'*'
    )
    # 指定允许的跨域请求方法
    CORS_ALLOW_METHODS = (
    	'DELETE',
    	'GET',
    	'OPTIONS',
    	'PATCH',
    	'POST',
    	'PUT',
    	'VIEW',
    )
    # 指定允许的跨域请求头
    CORS_ALLOW_HEADERS = (
    	'XMLHttpRequest',
    	'X_FILENAME',
    	'accept-encoding',
    	'authorization',
    	'content-type',
    	'dnt',
    	'origin',
    	'user-agent',
    	'x-csrftoken',
    	'x-requested-with',
    	'Pragma',
    )
    ```



### post请求携带数据

axios.post接收三个参数，第二个参数即参数对象，用{}包裹，构建要传递的参数键值对即可

```js
handleSubmit() {
      axios.post('http://127.0.0.1:8000/user/', {
        username: this.username,
        password: this.password
      }).then(res => {
        console.log(res.data)
        if (res.data.code == 100) {
          // 登录成功跳转到百度
          location.href = 'http://www.baidu.com'
        } else {
          alert(res.data.msg)
        }
      })
    }
```



### 携带请求头

axios.post接收三个参数，第三个参数即请求头，同样是对象类型

注意，请求头token需要在django-cors-headers的配置项CORS_ALLOW_HEADERS中

```python
# 请求头中带token

handleSubmit1() {
      axios.post('http://127.0.0.1:8000/user/',
          {
            username: this.username,
            password: this.password
          },
          {
            headers: {token: 'asdfasdfasd'}
          }
      ).then(res => {
        console.log(res.data)
        if (res.data.code == 100) {
          // 登录成功跳转到百度
          location.href = 'http://www.baidu.com'
        } else {
          alert(res.data.msg)
        }
      })
    }
```

## props配置项指定默认值

props是组件间通信，父传子，自定义属性时，子组件接收父组件传入的数据使用的配置项

### 基本使用

- 引入的路径
  - import Child from "@/components/Child";
  - @代表src目录
- 使用步骤
  - 父组件将属性值传入子组件
    - `<Child :msg="msg"></Child>`
  - 子组件
    - `props: ['msg'],`在props中接收，无需在data中再次定义

```vue
// Child.vue
<template>
  <div>
    <hr>
    <button>后退</button>
    {{ title }}----收到父组件传入的: {{ msg }}
    <button @click="handleClick">前进</button>
    <hr>
  </div>
</template>

<script>
export default {
  name: "Child",
  props: ['msg'],

  data() {
    return {
      title: '好看的首页'
    }
  },
  methods: {
    handleClick() {
      alert('前进')
    }
  }
}
</script>


<style scoped>
</style>
```

```vue
// App.vue
<template>
  <div id="app">
    <h1>我是App</h1>
    <h2>下面是局部组件</h2>
    <Child :msg="msg"></Child>

  </div>
</template>

<style>
h1 {
  background-color: red;
}
</style>

<script>
// import Child from "./components/Child";
import Child from "@/components/Child";  // @ 代指src路径
export default {
  name: 'App',
  data() {
    return {
      msg:'给儿子的一条消息'
    }
  },
  components: {
    Child
  }
}
</script>
```

### 限制类型

```javascript
props: {
    msg: String,
  },
```

补充：

- `<Child :msg="true"></Child>`接收到的msg是布尔类型，报错
- `<Child msg="msg"></Child>`接收到的msg是字符串，不报错

```vue
// App.vue
<template>
  <div id="app">
    <h1>我是App</h1>
    <h2>下面是局部组件</h2>
<!--    <Child :msg="true"></Child>-->
    <Child msg="true"></Child>

  </div>
</template>

<style>
h1 {
  background-color: red;
}
</style>

<script>
// import Child from "./components/Child";
import Child from "@/components/Child";  // @ 代指src路径
export default {
  name: 'App',
  data() {
    return {
      msg:'给儿子的一条消息'
    }
  },
  components: {
    Child
  }
}
</script>
```

```vue
// Child.vue
<template>
  <div>
    <hr>
    <button>后退</button>
    {{ title }}----收到父组件传入的: {{ msg }}
    <button @click="handleClick">前进</button>
    <hr>
  </div>
</template>

<script>
export default {
  name: "Child",
  props: {
    msg: String,
  },

  data() {
    return {
      title: '好看的首页'
    }
  },
  methods: {
    handleClick() {
      alert('前进')
    }
  }
}
</script>


<style scoped>
</style>
```



### 限定类型，限定必要性，限定默认值

```javascript
props: {
  msg: {
    type: String, //类型
    required: false, //必要性
    default: '老王' //默认值
  },
  xx:Boolean,
},    
```



```vue
// App.vue
<template>
  <div id="app">
    <h1>我是App</h1>
    <h2>下面是局部组件</h2>
    <Child></Child>

  </div>
</template>

<style>
h1 {
  background-color: red;
}
</style>

<script>
// import Child from "./components/Child";
import Child from "@/components/Child";  // @ 代指src路径
export default {
  name: 'App',
  data() {
    return {
      msg: '给儿子的一条消息'
    }
  },
  components: {
    Child
  }
}
</script>
```

```vue
// Child.vue
<template>
  <div>
    <hr>
    <button>后退</button>
    {{ title }}----收到父组件传入的: {{ msg }}
    <button @click="handleClick">前进</button>
    <hr>
  </div>
</template>

<script>
export default {
  name: "Child",
  props: {
    msg: {
      type: String, //类型
      required: false, //必要性
      default: '老王' //默认值
    },
  },

  data() {
    return {
      title: '好看的首页'
    }
  },
  methods: {
    handleClick() {
      alert('前进')
    }
  }
}
</script>


<style scoped>
</style>
```



## 混入

混入（minxin）：可以把多个组件共用的配置提取成一个混入对象

现在有一个App.vue和子组件Child.vue都有点击弹出name属性的handleShowName方法

```vue
// App.vue
<template>
  <div id="app">
    <h1>我是App</h1>
    <button @click="handleShowName">点我显示人名</button>

    <Child></Child>
  </div>
</template>

<style>
h1 {
  background-color: red;
}
</style>

<script>
// import Child from "./components/Child";
import Child from "@/components/Child";  // @ 代指src路径
export default {
  name: 'App',
  data() {
    return {
      name: 'lqz'
    }
  },
  methods: {
    handleShowName() {
      alert(this.name);
    }
  },
  components: {Child},
}
</script>
```

```vue
// Child.vue
<template>
  <div>
    <hr>
    <button @click="handleShowName">点我弹出名字</button>
    {{ title }}----
    <button>前进</button>
    <hr>


  </div>
</template>

<script>
export default {
  name: "Child",
  data() {
    return {
      title: '首页',
      name: '刘亦菲'
    }
  },
  methods: {
    handleShowName() {
      alert(this.name)
    }
  },
}
</script>

<style scoped>
</style>
```

如果有十个组件都有这个方法，我们需要重写10遍吗？显然不现实，而混入就是用来解决这个问题的，将该方法提取为一个混入对象

下面来看如何操作

第一步：抽取共用的代码，定义混入

```javascript
// mixin/index.js
export const hunhe = {
    methods: {
        handleShowName() {
            alert(this.name)
        }
    },
}
```

第二步：注册混入

### 全局注册

只要注册了，所有组件都可以用，在main.js中注册

```javascript
import {hunhe} from '@/mixin'
Vue.mixin(hunhe)
```

```javascript
// main.js
// 模块的导入
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

// 修改vue的提示信息，关闭开发模式
Vue.config.productionTip = false

import {hunhe} from '@/mixin'

Vue.mixin(hunhe)

// 不用动
new Vue({
    router,
    store,
    render: h => h(App)  // 等同于data，method等等
}).$mount('#app')  // 等同于el，与index.html中的div标签绑定

```



### 局部注册

只在被注册的组件中可用，在xx.vue中注册

```javascript
import {hunhe} from "@/mixin";
mixins: [hunhe]
```

```vue
// App.vue
<template>
  <div id="app">
    <h1>我是App</h1>
    <button @click="handleShowName">点我显示人名</button>

    <Child></Child>
  </div>
</template>

<style>
h1 {
  background-color: red;
}
</style>

<script>
// import Child from "./components/Child";
import Child from "@/components/Child";  // @ 代指src路径
import {hunhe} from "@/mixin";

export default {
  name: 'App',
  data() {
    return {
      name: 'lqz'
    }
  },
  methods: {},
  components: {Child},
  mixins: [hunhe],
}
</script>
```

```vue
// Child.vue
<template>
  <div>
    <hr>
    <button @click="handleShowName">点我弹出名字</button>
    {{ title }}----
    <button>前进</button>
    <hr>


  </div>
</template>

<script>
import {hunhe} from "@/mixin";

export default {
  name: "Child",
  data() {
    return {
      title: '首页',
      name: '刘亦菲'
    }
  },
  methods: {},
  mixins: [hunhe],
}
</script>


<style scoped>
</style>
```



## 插件

### 基本使用

用于增强Vue，后期咱们学的vuex，vue-router，elementui都是基于插件的形式使用的

下面我们来试着实现一个插件

#### 定义一个插件

```javascript
//src/plugins/index.js
import Vue from 'vue'

export default {
    install(a) {
        //参数a其实就是vue对象，因此下列代码的Vue可以用a代替
        console.log(a)
        //写代码，加入混入
        Vue.mixin({
            methods: {
                handleShowName() {
                    alert(this.name)
                }
            },
        })
        //写代码，给Vue对象上放变量
        Vue.prototype.xxx = 'qiu is nb'
        //写代码，给Vue对象上放函数
        Vue.prototype.hello = () => {
            alert("123")
        }
    }

}
```

#### 应用插件

在main.js中导入并使用插件

```javascript
// 模块的导入
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

// 修改vue的提示信息，关闭开发模式
Vue.config.productionTip = false

// 使用刚刚定义的插件
import plugins from "@/plugins";

Vue.use(plugins)//自动调用插件内的install，完成对Vue的强化

// 不用动
new Vue({
    router,
    store,
    render: h => h(App)  // 等同于data，method等等
}).$mount('#app')  // 等同于el，与index.html中的div标签绑定

```

此时的Vue对象就经过了强化，加入了混入以及添加了xxx属性以及hello方法

测试案例：

```vue
<template>
  <div id="app">
    <h1>我是App</h1>
    <button @click="handleShowName">点我显示人名</button>

    <Child></Child>
  </div>
</template>

<style>
h1 {
  background-color: red;
}
</style>

<script>
// import Child from "./components/Child";
import Child from "@/components/Child";  // @ 代指src路径

export default {
  name: 'App',
  data() {
    return {
      name: 'lqz'
    }
  },
  methods: {},
  components: {Child},
  created() {
    console.log(this.xxx)
    this.hello()
  }
}
</script>
```

### 进阶使用

- 可以在插件中将常用的第三方库导入，定义为一个属性，从而无需每一次在页面组件中使用都要重新导入
  - 通过插件，把axios对象放到vue实例上定义为`$http`属性
  - `Vue.prototype.$http = axios`
  - 以后要想使用axios发请求，无需导入，直接使用自己的`$http`属性即可
  - `this.$http.get()`
- 自定义指令 v-xx
- 定义很多全局组件
  - 通过在插件中定义全局组件，导入插件后，可以在所有的vue中直接使用组件
  - element-ui就是通过这种方式实现的，在main.js中导入并使用ElementUI，就可以直接使用它定义好的美观的组件



## scoped样式

当我们在某一组件中定义了style属性，默认该组件中的子组件也会应用该属性，造成污染

scoped样式就是为了解决这一污染问题，加在组件的style标签内，表示写的样式，只在当前组件生效，不会影响到其他组件

```vue
<style scoped>
  ...
<style>
```



## 具名插槽的其他写法

在前面的学习中，我们使用了具名插槽

```html
<div slot="bottom">
    <form action="">
        <p>用户名：<input type="text"></p>
        <p>密码：<input type="password"></p>
        <button>登录</button>
    </form>
    <p>我是p标签</p>
<div/>
```

如我们所知，必须将所有标签包裹在一个div里，否则无法同时将form表单与p标签插入到插槽当中

但这样生成的前端页面中也会有外层div的存在，也没有方式可以不要最外层的div也可以将两个标签作为整体插入到插槽中呢？

我们可以使用`template`标签，同时，官方推荐我们采用`v-slot:bottom`的语法来指定插槽

```html
<template v-slot:bottom>
    <form action="">
        <p>用户名：<input type="text"></p>
        <p>密码：<input type="password"></p>
        <button>登录</button>
    </form>
    <p>我是p标签</p>
</template>
```



## 项目中使用bootstrap，elementui

在使用Vue进行开发时有很多可以选择的前端组件库，帮助我们轻松构建美观的前端页面，常见的如下

- iView：https://www.iviewui.com/view-ui-plus/guide/introduce
- app端的ui组件库：http://vant3.uihtm.com/#/zh-CN
- BootStrapVue：https://code.z01.com/bootstrap-vue/

推荐使用elementui：https://element.eleme.cn/#/zh-CN/component/installation

### elementui的使用

- 安装

  - ```javascript
    npm i element-ui -S
    ```

- 在main.js中引入

  - ```javascript
    import Vue from 'vue';
    import ElementUI from 'element-ui';
    import 'element-ui/lib/theme-chalk/index.css';
    import App from './App.vue';
    
    Vue.use(ElementUI);
    
    new Vue({
      el: '#app',
      render: h => h(App)
    });
    ```

  - 完整的main.js文件如下

  - ```javascript
    // 模块的导入
    import Vue from 'vue'
    import App from './App.vue'
    import router from './router'
    import store from './store'
    
    // 修改vue的提示信息，关闭开发模式
    Vue.config.productionTip = false
    
    //使用elementui
    import ElementUI from 'element-ui';
    import 'element-ui/lib/theme-chalk/index.css';
    
    Vue.use(ElementUI);
    
    // 不用动
    new Vue({
        router,
        store,
        render: h => h(App)  // 等同于data，method等等
    }).$mount('#app')  // 等同于el，与index.html中的div标签绑定
    
    ```

- 使用

  - 直接去官网复制代码，贴进去即可
  - 注意复制的完整性，包括templage，script，style内容



### bootstrap的使用

因为bootstrap使用了jQuery的内容，因此要想使用bootstrap，还需要引入jQuery

- 安装

  - cnpm install jquery
  - cnpm install bootstrap@3

- 在main.js中引入

  - ```javascript
    import 'bootstrap'
    import 'bootstrap/dist/css/bootstrap.min.css'
    ```

- 在vue的配置文件中vue.config.js中配置jQuery

  - ```javascript
    const {defineConfig} = require('@vue/cli-service')
    const webpack = require("webpack");
    module.exports = {
        configureWebpack: {
            plugins: [
                new webpack.ProvidePlugin({
                    $: "jquery",
                    jQuery: "jquery",
                    "window.jQuery": "jquery",
                    "window.$": "jquery",
                    Popper: ["popper.js", "default"]
                })
            ]
        }
    };
    ```

  - 配置完成后的vue.config.js文件如下

  - ```javascript
    const { defineConfig } = require('@vue/cli-service');
    const webpack = require("webpack");
    
    module.exports = defineConfig({
        configureWebpack: {
            plugins: [
                new webpack.ProvidePlugin({
                    $: "jquery",
                    jQuery: "jquery",
                    "window.jQuery": "jquery",
                    "window.$": "jquery",
                    Popper: ["popper.js", "default"]
                })
            ]
        },
        transpileDependencies: true
    });
    ```

- 直接使用bootstrap样式，复制黏贴即可



## localStorage和sessionStorage

前端存储数据的地方

- cookie中：借助第三方插件，自己用js写
- sessionStorage：关闭浏览器自动销毁
- localStorage：永久存在，除非手动删除，或浏览器存满了

通过这三个东西就可以实现组件间通信

```javascript
# 使用：sessionStorage
sessionStorage.setItem('name', 'lqz')  // 存
sessionStorage.getItem('name')  // 取
sessionStorage.clear()  // 清空所有
sessionStorage.removeItem('name')  // 根据键删除

# 使用：localStorage
localStorage.setItem('name', 'lqz')
localStorage.getItem('name')
localStorage.clear()
localStorage.removeItem('name')
```

某些不需要登陆就可以添加购物车的网站就是使用localStorage实现的，无需登录，数据存在本地，下次打开浏览器购物车数据还在

## vuex

vuex：状态管理器，存取数据用的,可以跨组件通信（屏蔽了组件的父子关系）

每一个Vuex应用的核心就是Store（仓库），我们可以说Store是一个容器，Store里面的状态与单纯的全局变量是不一样的，无法直接改变Store中的状态。想要改变状态需通过mutation去修改。

### vuex具体使用方法

#### store的创建

> 在 src根目录下创建文件夹store，并在其下index.js。需包含actions，mutations，state结构如下：

```javascript
// 引入vue
import Vue  from 'vue'
 
// 引入vuex
import Vuex from 'vuex'
 
// 应用vue插件
Vue.use(Vuex)
 
// actions响应组件中的动作
const actions = {
}
 
// mutations操作数据state
const mutations = {
}
 
// 准备state存储数据
const state = {
   //状态对象
}
 
// 创建store并导出
const store = new Vuex.Store({
        actions,
        mutations,
        state,
})
 
 
//默认导出store
export default store
```

#### 引入store

在main.js中引入store，全局组件都可以使用vuex

```javascript
import store from './store'
new Vue({
    router,
    store,  # 把store放在这里
    render: h => h(App)
}).$mount('#app')
```

### 各个状态的核心概念

![img](/post/Vue/Vue2/eb9c566732fb3100f4a3f27a65983e09.png)

#### state

state是状态数据，可以通过this.$store.state来直接获取状态，也可以利用vuex提供的mapState辅助函数将state映射到计算属性（computed）中去。

例如，定义了如下state

```javascript
const state = {
	sum:0,
	realname:'张三',
	age:19
}
```

可以通过插值引用直接取出state中的值：`{{$store.state.sum}}`

#### mutations

更改 Vuex 的 store 中的修改状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的事件类型 (type)和一个回调函数 (handler)。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数。

例如，我们定义了如下mutations

```javascript
// 操作state中的数据
mutations :{
    editTitle(state,value){
        state.title = value;
    },
    editSinger(state,value){
        state.singer = value;
    }
},
```

使用methods方法，使用commit直接操作state中的数据

```javascript
<button @click="editTitle('Lover')">更改歌名</button>
 
editTitle(str){
    this.$store.commit('editTitle',str);
},
```

#### actions

Action 提交的是 mutation，而不是直接变更状态。

例如，我们定义了如下actions

```javascript
// 相应组件中的动作，组件中可以通过dispatch发送，
// actions 通过commit发送动作告知mutations
actions:{
	changeSing(context,value){
        setTimeout(()=>{
            context.commit('editTitle',value);
                },3000)
    },
	changeSinger(context,value){
        setTimeout(()=>{
            context.commit('editSinger',value);
                },3000)
    }
},
```

使用methods方法，调用dispatch，传递mutation名与value，mutation再调用commit，传递回调函数以及value，真正实现state数据的修改

```javascript
 <button @click="changeSing('EndGame')">三秒后更改歌名</button>
 
    changeSing(str){
        setTimeout(()=>{
            this.$store.dispatch('changeSing',str);
        },3000)
    },
     changeSinger(str){
        setTimeout(()=>{
            this.$store.dispatch('changeSinger',str);
        },3000)
    }
```



### 总结

在任意组件中，要获取state中的数据，只需要`this.$store.state.变量`即可

要想修改state中的数据，有两种实现方式

- 直接调用commit
  - this.$store.commit("motations中定义的函数"，参数)
  - motations中定义的函数实现数据的变化
- 调用dispatch
  - this.$store.dispatch("actions中定义的函数",参数)
  - actions定义的函数进一步触发commit
  - motations中定义的函数实现数据的变化

推荐使用dispatch，因为它更加安全，可以在里面进行权限校验，向后端发请求判断是否commit此次修改数据的请求

## vue-router

spa:单页面应用，即只有一个主页面，页面的不同依赖的是组件的来回切换，需要借助于vue-router

### vue-router的具体使用

#### 创建路由配置表

在项目的src目录下新建routes文件夹，创建index.js文件来统一管理路由配置表

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
//引入vue组件
import Home from '@/views/Home'
import AboutView from '@/views/AboutView'

Vue.use(VueRouter)


// 注册路由，配置路由与vue组件的对应关系
const routes = [
    {
    path: '/home',
    name: 'home',
    component: Home
  },
    {
    path: '/about',
    name: 'about',
    component: AboutView
  },
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router
```

#### 在main.js中注册

```javascript
import router from './router'

new Vue({
    router,
    render: h => h(App)
}).$mount('#app')
```

#### 组件中使用路由

router-link 是 vue-router 提供的一个全局组件，它是用来做路由跳转，底层其实就是一个a标签

router-view 是 vue-router 提供的一个全局组件，它是用来提供一个路由占位，用来挂载 URL 匹配到的组件。

App.vue文件

```vue
<template>
  <div id="app">
    <router-link to="/about">Go to /about</router-link>

    <router-view></router-view>
  </div>
</template>

<style>

</style>

<script>

export default {
  name: 'App',
  data() {
    return {}
  },
}
</script>
```

#### 使用

- 写好页面组件
- 在router/index.js中引入组件并配置路由与组件的对应关系
- 在main.js中注册
- 在单页面中插入路由占位`<router-view></router-view>`

```
以后访问路径
http://192.168.1.5:8080/home   显示Home组件
http://192.168.1.5:8080/about  显示AboutView组件
```

在任意组件中，`this.$router`，拿到的就是导出的router对象





## 总结

```
# 前端发展历史
	-render返回一个一个页面---》ajax出现---（ajax+dom）实现局部刷新--》前端写很多页面html，js，css---》vue，react---》一套代码，处处运行(谷歌，uni-app)
# vue的介绍
	-js框架，vue2     vue3 
    -MVVM架构： m：model(js中数据)   v：view  vm:ViewModel   页面变，数据跟着变，数据变页面也变化
    -组件化开发
    -单页面开发---》页面间的跳转(vue-router)
# vue的快速使用
	-引入vue，new Vue实例对象，传入一些配置项data,methods，在被vue管理的标签中使用vue语法
# 插值语法
	-{{变量，简单的表达式，函数}}
    -三目运算符---》python中的三元表达式
# 指令v-html和v-text
	-v-xx 写法，写在标签上的，统称为vue的指令 
    -<p v-html=''></p>    xss攻击
    -v-text
# v-if和v-show
	-控制标签显示与隐藏
# 双向数据绑定演示
	
# v-on 事件指令
	-点击事件，双击事件，失去焦点。。。。
    -@事件名='函数'
	-v-on:事件名='函数'，methods:{  handleClick(){},}
    -es6的对象写法
  	    {变量，变量}
        {函数名(){},}
# 属性指令v-bind
	-标签上的属性可以动态变化---》js的变量
    -任意标签的任意属性上：name，id，class，style，src，href，自己定义的属性
    -<p id='id_p' class='red' :aa='aaaaa'></p>
    -img：src，a：href属性
    -v-bind:src=''
    -简写:src=''
# style和class的绑定
	-class和style比较特殊的属性，控制样式
    -:class= 字符串(空格表示多个类)，数组(合适)，对象(key值是类名，value值是布尔值)
    -:style= 字符串，数组(对象)，对象
     -css中两个单词是用 font-size---》fontSize
    -<p :style='{fontSize="30px"}'></p>
# 条件渲染
	-v-if=条件   v-else-if=条件   v-else
# 列表渲染v-for
	-放在标签上，循环多少次，标签就会显示多少次
    -v-for='item in 数组，对象，数字'
# 购物车显示不显示小案例
# v-model的使用
	-针对于input标签 :value='变量'，单向数据绑定
	-双向数据绑定：v-model='变量'
# blur，change，input事件
	-input标签的 失去焦点，变化，输入内容就变化的事件
# 过滤案例
# 事件修饰符
	-按钮只能点击一次
    -只响应自己对事件
    -阻止a标签跳转
   	-阻止事件冒泡
# 按键修饰符
	-@keyup="handleKey1"---》监控到用户点击那个键
```

表单控制

- v-model绑定：双向数据绑定
- checkbox：单选:布尔和多选:数组
- radio：单选:字符串

购物车案例

- v-for循环
- 插值语法放函数显示返回值： {{getprice()}}
- js循环的几种方式
- checkbox的多选和单选
- input的change事件
- 数量加减（插值语法可以直接放表达式）

v-model进阶

- lazy，number，trim

vue声明周期

- vue对象(vm)，vuecompoment对象(vc)
- 8个生命周期钩子函数，依次执行
- created:有数据了，在此发送ajax请求，设置一些定时任务
- destroyed：资源请理性的工作（清空定时器）
- 定时任务（setInterval），延迟任务（setTimeout）

前后端交互之jq的ajax

- jquery的ajax

- fetch和axios发送请求

  - 补充：箭头函数

  - axios发送get请求

    - axios.get(url)

  - axios发送post请求（请求头携带数据）

    - axios.post(url,{name:'lqz',age:19})
    - axios.post(url,json.stringify({name:'lqz',age:19}))

  - axios发送请求，请求头中带数据（jwt，认证session）

    - ```
      axios.post(url, data, {
              headers: {
                  'token':'asdfads'
              }
      })
      ```

    - 

显示电影小案例

- 前后端交互的编码格式
  - urlencoded：form表单提交默认的格式，ajax提交默认格式
  - json:前后端分离项目常用的格式
  - form-data：上传文件使用这种格式

计算属性

- 插值写函数，页面只要刷新，函数会重新运算，耗费资源

- ```
  computed:{
      fucName(){}
  }
  ```

- 有缓存，直接把fucName当变量用，只有fucName函数的参数变化才重新运算

监听属性

- 只要被监听数据发送变化，就会触发某个函数的执行

- ```
  watch:{
      name(){
        只要name属性发生变化，这个函数就会执行
      }
  }
  ```

- 案例：过滤案例中输入内容变化就改变list属性的值

局部和全局组件的定义

自定义属性实现父传子

- 在子组件上自定义的属性，传入父组件的值

- 在子组件中的props定义属性

- ```
  <navbar :myname="name" :age="age"></navbar>
  
  props: ['myname', 'age']
  ```

自定义事件实现子传父

- this.$emit(事件名)
- 将子组件的值传递到父组件的事件触发的函数中

ref属性实现组件间通信

- 放在普通标签，放在组件上
- this.$refs.refname即可拿到标签对象或组件对象

动态组件

- component标签通过is决定显示的组件是什么

插槽的使用

- 不具名插槽
- 具名插槽
  - 指定slot属性值

vue-cli创建项目并启动

vue项目目录介绍
