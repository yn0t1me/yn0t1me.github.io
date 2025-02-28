---
title: "Vue3"
slug: "Vue3"
description: "介绍Vue3的相关知识"
date: "2025-02-27T19:08:25+08:00"
lastmod: "2025-02-27T19:08:25+08:00"
image: 
categories: ["前端"]
tags: ["Vue"]
---


<!--more-->

## Vue3介绍

vue3比vue2好，性能提高了，支持ts语法，源码升级，多个一些新的api

最主要的区别是vue3使用组合式API，而vue2使用的配置项API

vue2的所有语法可以完全迁移到vue3，完全兼容

## Vue3项目创建

方式一：使用vue-cli创建

创建步骤同vue2，只是版本选项由vue2改选vue3，不再赘述

方式二：使用vite创建

- 创建工程
  - `npm create vite@latest my-vue3-project --template vue`
- 安装依赖
  - `cd vue3_test2`
  - `npm install`
- 运行
  - `npm run dev`

安装过程中询问是否安装create-vite，同意即可，选择前端框架为vue即可

> vite创建的项目如要使用vuex或vue-router，需要自己创建对应文件夹，完成配置即可



## 常用api之setup

以往在组件中使用的是配置项API，即data，methods等等，现在vue3推荐我们使用组合式API，即将所有的属性，方法等等都写在setup方法中，然后通过return返回出去即可被使用（原先的配置项API依然支持）

```js
<template>
  <!--组件不需要放在一个标签内了-->
  {{ x }}
  <br>
  <h2>{{ name }}==={{ age }}</h2>
  <button @click="handleAdd">点我年龄+1</button>
  <hr>

</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>

<script>
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data() {
    return {
      x: 100
    }
  },
  //使用组合式API
  setup() {
    // 定义变量
    let name = 'lqz'
    let age = 19

    // 定义函数
    function handleAdd() {
      age = age + 1    // 数据变了，页面没变
      console.log(age)
    }

    // 要在模版中使用必须，return出去
    return {name, age, handleAdd}
  }
}
</script>
```

但是有一个问题，会发现点击后age的值发生变化，但页面没有相应的发生变化

## 常用api之ref和reactive

要实现响应式，需要对数据进行处理

基本数据类型使用ref包裹一下，使之变成响应式（变量变成了一个对象）

- 在模版中可以直接使用
- 在setup中使用对象.value才能获取真正的值并对其进行操作

对象，数组类型，使用reactive包裹

- 在模版和setup中都可以直接使用

```vue
<template>
  <!--组件不需要放在一个标签内了-->
  {{ x }}
  <br>
  <h2>{{ name }}==={{ age }}</h2>
  <button @click="handleAdd">点我年龄+1</button>
  <hr>
  <h2>姓名是：{{ person.name }}，年龄是：{{ person.age }}</h2>
  <button @click="handleChange">点我修改刘亦菲的名字</button>
  <hr>


</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>

<script>
import HelloWorld from "@/components/HelloWorld.vue";
import {ref, reactive} from "vue";

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data() {
    return {
      x: 100
    }
  },
  //使用组合式API
  setup() {
    // 定义变量
    let name = ref('lqz')
    let age = ref(19)
    let person = reactive({
      name: '刘亦菲',
      age: 19
    })

    // 定义函数
    function handleAdd() {
      age.value = age.value + 1    // 数据变了，页面没变
      console.log(age)
    }

    function handleChange() {
      person.name += '？'
    }

    // 要在模版中使用必须，return出去
    return {name, age, handleAdd, person, handleChange}
  }
}
</script>
```



## 计算和监听属性

在vue2中我们学习了计算属性和监听属性的配置项API的写法，在vue3的组合式API中，我们又该怎么写？

> 组合式API的好处：
>
> 	将实现一个功能的属性，方法放在一起，易于阅读和维护

```vue
<template>
  <h2>{{ fullname }}</h2>
  <br>
  <button @click="handleChange">点我改名</button>
  <br>
  <h2>sum:{{ sum }}</h2>
  <button @click="sum++">点我sum+1</button>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>

<script>

import {ref, reactive, computed, watch} from "vue";

export default {
  name: 'App',
  setup() {
    const sum = ref(10)
    const person = reactive({
      firstName: '邱',
      lastName: '伟杰'

    })
    let fullname = computed(() => {
      return person.firstName + person.lastName;
    })

    function handleChange() {
      person.firstName = '王'
    }

    watch(sum, (newValue, oldValue) => {
      console.log("老数据：", oldValue)
      console.log("新数据：", newValue)
    })
    return {person, fullname, handleChange, sum}
  }
}
</script>
```



## 生命周期

vue3的生命周期钩子函数与vue2稍有不同。

vue3的生命周期钩子函数需要从 `vue` 库中导入，并在 `setup()` 函数中使用

`setup()` 函数的执行时机相当于选项式 API 中的 `beforeCreate` 和 `created` 的组合，其他钩子函数以函数的形式写在setup中，并传入一个箭头函数

```python
beforeCreate===>setup()
created=======>setup()
beforeMount ===>onBeforeMount
mounted=======>onMounted
beforeUpdate===>onBeforeUpdate
updated =======>onUpdated
------下面的变了------
beforeUnmount ==>onBeforeUnmount
unmounted =====>onUnmounted
```

```vue
<template>
  <div>
    <h1>{{ message }}</h1>
    <button @click="increment">Increment</button>
  </div>
</template>

<script>
import { ref, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue';

export default {
  setup() {
    const message = ref('Hello, Vue 3!');
    const count = ref(0);

    // 挂载前
    onBeforeMount(() => {
      console.log('Before Mount: Component is about to be mounted');
    });

    // 挂载后
    onMounted(() => {
      console.log('Mounted: Component has been mounted');
    });

    // 更新前
    onBeforeUpdate(() => {
      console.log('Before Update: Component is about to be updated');
    });

    // 更新后
    onUpdated(() => {
      console.log('Updated: Component has been updated');
    });

    // 卸载前
    onBeforeUnmount(() => {
      console.log('Before Unmount: Component is about to be unmounted');
    });

    // 卸载后
    onUnmounted(() => {
      console.log('Unmounted: Component has been unmounted');
    });

    // 自定义方法
    const increment = () => {
      count.value++;
    };

    return { message, count, increment };
  }
};
</script>
```



## toRefs

es6语法中的对象解压，使用后可以简化模板中的使用，无需使用`对象.属性`，可以直接通过属性调用

```js
// es6语法中的对象解压
return {
  ...toRefs(data)  // 解压开，return了
  // name:data.name,
  // age:data.age,
  // isShow:data.isShow,
}
```

完整代码：

解压前

```vue
<template>
  <h2>{{ person.name }}==={{ person.age }}</h2>

</template>


<script>

import {ref, reactive, computed, watch} from "vue";

export default {
  name: 'App',
  setup() {
    const person = reactive({
      name: '邱伟杰',
      age: 17
    })
    return {person}
  }
}
</script>
```

解压后

```vue
<template>
  <h2>{{ name }}==={{ age }}</h2>

</template>


<script>

import {ref, reactive, toRefs} from "vue";

export default {
  name: 'App',
  setup() {
    const data = reactive({
      name: '邱伟杰',
      age: 17
    })
    return {
      // es6语法中的对象解压
      ...toRefs(data)  // 解压开return，相当于
      // name:data.name,
      // age:data.age,
    }
  }
}
</script>
```

## 回顾和扩展



### 常见ES6语法



#### 变量和常量

对于var，let，const三个声明变量的关键字，他们的主要区别如下

- var

  - 作用域：var 声明的变量是函数作用域（function-scoped），即在函数内部声明的变量只能在函数内部访问。

  - 提升（Hoisting）：var 声明的变量会被提升到其所在作用域的顶部，这意味着你可以在声明之前访问变量，但其值为 undefined。

  - 重复声明：在同一个作用域内，var 允许重复声明同一个变量

- let
  - 作用域：let 声明的变量是块作用域（block-scoped），即在 {} 内声明的变量只能在该块内访问
  - 提升（Hoisting）：let 声明的变量也会被提升，但在声明之前访问会抛出 ReferenceError，形成所谓的“暂时性死区”（Temporal Dead Zone, TDZ）
  - 重复声明：在同一个作用域内，let 不允许重复声明同一个变量
- const
  - 作用域：const 声明的变量也是块作用域（block-scoped）
  - 提升（Hoisting）：let 声明的变量也会被提升，但在声明之前访问会抛出 ReferenceError，形成所谓的“暂时性死区”（Temporal Dead Zone, TDZ）
  - 重复声明：在同一个作用域内，const 不允许重复声明同一个变量
  - 不可变性：const 声明的变量必须初始化，且不能重新赋值。但对于对象和数组，虽然变量本身不能重新赋值，但其属性或元素可以修改

```html

<script>
    function show(){
        if(1==1){
            var name = "武沛齐";  //函数作用域=Python
        }
        // 函数作用域，可以访问到name
        consolo.log(name);
    }
    show();
</script>
```

```html
<script>
    if(1==1){
    	let age = 19;  // 块级作用域
    }
    // 块级作用域，访问不到age
    console.log(age);
</script>
```

```html
<script>
    if(1==1){
    	const age = 19;  // 块级作用域 + 常量
    }
    // 块级作用域，访问不到age
    console.log(age);
</script>
```

```html
<script>
    const info = {id:1,value:18};
    // info本身不能够进行赋值，但其属性或元素可以修改，这也是为什么用ref对一个const定义的变量包裹后，可以修改它的value值
    info.value = 999;
</script>
```



#### 2.模板字符串

在es5的语法中做字符串拼接需要使用加号实现

```javascript
let info = "我是" + "?" + "今年技术";
```

在es6中，有了模板字符串

模板字符串使用反引号来定义字符串，提供字符串拼接、嵌入表达式、多行字符串

- 表达式需要用 `${}` 包裹，然后在模板字符串中直接插入变量或计算结果
- 模板字符串支持多行字符串，不需要使用 `\n` 来换行
- 还可以嵌入标签

```html
<script>
    let name = "张开";
    let age = 73;
    
	let info =  `我叫${name}，今年${age}岁`;
</script>
```



#### 3. 动态参数

剩余参数（Rest Parameters）data是数组类型，可以接收多余的所有参数：

- 只能出现在函数参数列表的最后一位
- 它会将函数调用时传入的多余参数收集到一个数组中

```html
<script>
	function info(v1,...data){
        console.log(v1,data);
    }
	info(11);
    info(11,22);
    info(11,22,333,444,55);
</script>
```

![image-20250208144832691](/post/Vue/Vue3/image-20250208144832691.png)

扩展运算符（Spread Operator）用于将数组或对象展开为独立的元素

```html
<script>
	function info(v1,v2,v3,v4){
        console.log(v1,v2,v3,v4);
    }

    info(11,22,333,444);
    nums = [22,33,44,55,66,77,88];
    info(11,...nums)
</script>
```

![image-20250208145000810](/post/Vue/Vue3/image-20250208145000810.png)



#### 4.解构赋值

解构赋值允许我们将数组或对象中的值快速提取到独立的变量中，主要包括数组解构、对象解构以及一些高级用法

- 数组解构

```html
<script>
    let nums = [11,22,33,44];
    // 基本用法
    let [n1,n2] = nums;
    console.log(n1); // 输出：11
	console.log(n2); // 输出：22
    // 跳过某些值
    let [n1,,n2] = nums;
    console.log(n1); // 输出：11
	console.log(n2); // 输出：33
    // 提取剩余值
    let [n1,...n2] = nums;
    console.log(n1); // 输出：11
	console.log(n2); // 输出：[22,33,44]
    // 默认值
    let nums = [11,22];
    let [n1,n2,n3=33] = nums;
    console.log(n1); // 输出：11
	console.log(n2); // 输出：22
    console.log(n3); // 输出：33
</script>
```

搭配函数传参

```html
<script>
    function getData(n1,[n2,n3,n4]){
        console.log(n1,n2,n3,n4)
    }
    let nums = [11,22,33,44];
    getData(100,nums);
</script>
```

- 对象解构

```html
<script>
	let info = {name: "Kimi", age: 25, city: "Shanghai"};
    // 基本用法
	let {name,age,addr} = info;
    console.log(name);
    console.log(addr);
    // 提取部分属性
    let {name,addr} = info;
    console.log(name);
    console.log(addr);
    // 重命名变量
    let { name: fullName, age: years } = person;
    console.log(fullName);
	console.log(years);
	// 默认值
    { name, age, city, sex="male" } = person;
    console.log(city);
</script>
```

同样的，配合函数传参

```html
<script>
    
    function getData(n1,{name,addr}){
    	//let {name,addr} = info; 
	    console.log(name);
		console.log(addr);
    }
    
    let info = {name:"武沛齐",email:"wupeiqi@live.com",addr:"北京"};
    getData(111,info);
	
</script>
```

Vue3中需要什么就要导入什么，不像vue2中可以直接去this对象中获取，`this.$router` `this.$route`。

```
import {name,addr} from 'vue'
```



#### 5.箭头函数

注意：发送网络请求时，回调函数必须要使用箭头函数。（规范统一）

- 语法差异
  - 使用 `=>` 定义
  - 如果函数体只有一条语句，可以省略大括号 `{}`，并且自动返回结果
  - 不能有函数名（匿名函数）
  - 传参
    - 如果没有参数，可以使用一对空括号 `()` 来表示参数列表
    - 如果只有一个参数，可以省略参数周围的括号 `()`，直接写参数名

```html
<script>
	function add(a, b) {
  		return a + b;
	}
    f1(11,22);
	
    //箭头函数
    const f2 = (a, b) => a + b; // 简洁形式
    f2(11,22);
</script>
```

- this关键字
  - 普通函数的 `this` 是动态绑定的，取决于函数的调用方式
    - 作为方法调用：`this` 指向调用它的对象
    - 作为普通函数调用：`this` 指向全局对象（在严格模式下是 `undefined`）
  - 箭头函数没有自己的 `this`，它会捕获其所在上下文的 `this` 值。换句话说，箭头函数的 `this` 是在其定义时确定的，而不是在调用时确定的


```html
<script>
    var name = "源代码";
    let info = {
        name: "武沛齐",
        func: function () {
            console.log(this.name); // this执行调用它的对象即this=info，输出武沛齐
        }
    }
    info.func(); //函数作为方法调用

    function getData() {
        console.log(this.name); // this指向全局对象，this=window，输出源代码
    }

    getData(); //作为普通函数调用
</script>
```

调整：结果依然不变

因为作为普通函数调用时，this指向全局对象

```html
<script>
    var name = "源代码";
    let info = {
        name: "武沛齐",
        func: function () {
            console.log(this.name); // 函数内部默认都有this关键字,this=info对象

            function getData() {
                console.log(this.name); // 函数内部默认都有this关键字，this=window
            }
            getData();
        }
    }
    info.func();
</script>
```

但如果是箭头函数就不一样了，箭头函数没有自己的 `this`，它会捕获其所在上下文的 `this` 值。在箭头函数定义时，是处在function函数内，此时this=info对象，所以箭头函数内的this=info对象

```html
<script>
    var name = "源代码";
    let info = {
        name: "武沛齐",
        func: function () {
            console.log(this.name); // 函数内部默认都有this关键字,this=info对象

            let getData = () => {
                console.log(this.name);
            }
            getData();
        }
    }
    info.func();
    
</script>
```

在vue2中，如果回调函数使用普通函数则会出现问题，就是this指向问题导致的

案例如下（伪代码，模拟vue2项目中发生网络请求）：

因为回调函数是普通函数，且是作为普通函数被调用而不是方法，因此该this实际上是window对象而不是我们期望的vue对象，从而导致vue对象中的data无法被正确的赋值

```html
<script src="https://cdn.bootcdn.net/ajax/libs/axios/0.21.1/axios.min.js"></script>
<script>

    let info = {
        data: null,
        send: function () {
            axios({
                method: "post",
                url: "https://api.luffycity.com/api/v1/auth/password/login/?loginWay=password",
                data: {
                    username: "alex",
                    password: "dsb"
                },
                headers: {
                    "content-type": "application/json"
                }
            }).then(function (res) {
                console.log(this,res);
                this.data = res;
            })
        }
    }
    info.send();
</script>
```

如果使用箭头函数，就可以避免这个问题，它的this依赖于上下文，此时就是我们期望的vue对象，从而正确的对data进行赋值

```html
<script src="https://cdn.bootcdn.net/ajax/libs/axios/0.21.1/axios.min.js"></script>
<script>

    let info = {
        data: null,
        send: function () {
            axios({
                method: "post",
                url: "https://api.luffycity.com/api/v1/auth/password/login/?loginWay=password",
                data: {
                    username: "alex",
                    password: "dsb"
                },
                headers: {
                    "content-type": "application/json"
                }
            }).then((res) => {
                console.log(this,res);
                this.data = res;
            })
        }
    }
    info.send();
</script>
```

#### 6.模块的导入导出

- 模块的导出
  - 命名导出（Named Export）
  - 默认导出（Default Export）
- 模块导入
  - 命名导入（Named Import）
  - 默认导入（Default Import）
  - 命名空间导入（Namespace Import）

```html
<script>
    // mainComponent.js
    // 命名导出
	export const add = (a, b) => a + b;
	export const subtract = (a, b) => a - b;
	export const multiply = (a, b) => a * b;
    // 等价于
    const add = (a, b) => a + b;
	const subtract = (a, b) => a - b;
	const multiply = (a, b) => a * b;

	export { add, subtract, multiply };
    // 默认导出，可以是一个函数、类、对象等
    export default app; // 默认导出
</script>
```

```html
<script>
    // main.js
    // 命名导入
	import { add, subtract } from './mathUtils.js';
    // 默认导入，可以通过任意名称导入
    import app from './mainComponent.js';
    // 命名空间导入，将一个模块中的所有导出内容作为一个对象导入
    import * as MathUtils from './mathUtils.js';
</script>
```

命名导出+命名导入

![image-20221113100640595](/post/Vue/Vue3/image-20221113100640595.png)

命名导出+命名空间导入

![image-20221113100708949](/post/Vue/Vue3/image-20221113100708949.png)

默认导出+默认导入

![image-20221113100834135](/post/Vue/Vue3/image-20221113100834135.png)

### 关于环境

- 关于node.js
  ![image-20221113102414428](/post/Vue/Vue3/image-20221113102414428.png)

  
  
- 关于npm

  ```
  sudo npm install npm@latest -g
  ```

  ```
  npm config set registry https://registry.npm.taobao.org
  
  npm install -g cnpm --registry=https://registry.npm.taobao.org
  ```

- vue-cli

  ```
  # 安装（最新版）
  sudo npm install -g @vue/cli
  
  # 安装（指定版本）
  sudo npm install -g @vue/cli@5.0.8
  
  # 卸载
  sudo npm uninstall -g @vue/cli
  ```

  ```
  @vue/cli@5.0.8
  ```

  

### 创建项目

- 命令行创建
- WebStorm

使用WebStorm创建项目，自动配置好项目启动按钮等，但是vuex，vue-router等组件需要手动安装，其他均已配置好

![image-20250208181420027](/post/Vue/Vue3/image-20250208181420027.png)

### 项目部署

具体步骤如下：

第一步：编译

```js
npm run build
```

- 在编译之前，要确保所有的依赖都已正确安装

- ```js
  npm install
  ```

- 将项目打包成静态文件，通常输出到 `dist` 文件夹中

- ![image-20250208182317285](/post/Vue/Vue3/image-20250208182317285.png)

第二步：将编译后的代码dist文件上传到服务器（阿里云、腾讯云）

![image-20220312123432759](/post/Vue/Vue3/image-20220312123432759.png)

第三步：安装nginx + 配置 + 启动

```
yum install nginx
```


  `vim /etc/nginx/nginx.conf`进入nginx的配置文件配置项目目录root

  ```
  user nginx;
  worker_processes auto;
  error_log /var/log/nginx/error.log;
  pid /run/nginx.pid;
  
  # Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
  include /usr/share/nginx/modules/*.conf;
  
  events {
      worker_connections 1024;
  }
  
  http {
      log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                        '$status $body_bytes_sent "$http_referer" '
                        '"$http_user_agent" "$http_x_forwarded_for"';
  
      access_log  /var/log/nginx/access.log  main;
  
      sendfile            on;
      tcp_nopush          on;
      tcp_nodelay         on;
      keepalive_timeout   65;
      types_hash_max_size 4096;
  
      include             /etc/nginx/mime.types;
      default_type        application/octet-stream;
  
      include /etc/nginx/conf.d/*.conf;
  
      server {
          listen       80;
          listen       [::]:80;
          server_name  _;
          # 项目目录
          root         /data/mysite;
  
          # Load configuration files for the default server block.
          include /etc/nginx/default.d/*.conf;
  
          error_page 404 /404.html;
          location = /404.html {
          }
  
          error_page 500 502 503 504 /50x.html;
          location = /50x.html {
          }
      }
  }
  ```

重启nginx

  ```
  systemctl restart nginx
  ```

第四步：访问
![image-20221112172411638](/post/Vue/Vue3/image-20221112172411638.png)





## flex布局

传统的页面布局：div+css+float实现。

flex布局（Flexible Box Layout）：实现更简单。

```html
<div class='menu'>
	<div class='item'>北京</div>
	<div class='item'>上海</div>
</div>
```

外层的div标签相当于一个容器，里面的两个div可视为该容器的元素

### 容器

#### 布局

将容器设置为flex布局

```html
<div class='menu'>
	<div class='item'>北京</div>
	<div class='item'>上海</div>
</div>

<style>
    .menu{
        display:flex;
    }
</style>
```

#### 2.主轴方向

```
flex-direction: row 	主轴横向
flex-direction: column	主轴纵向
```



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .menu {
            border: 1px solid red;
            width: 500px;

            display: flex;
            flex-direction: row; /*主轴=横向*/
        }
    </style>
</head>
<body>
<div class='menu'>
    <div class='item'>北京</div>
    <div class='item'>上海</div>
</div>


</body>
</html>

```

#### 3.元素对齐方式

```
justify-content: 定义子元素在主轴上的对齐方式
align-items: 定义子元素在交叉轴上的对齐方式
align-content: 定义多行子元素在交叉轴上的对齐方式，实现多行控制
```

对齐方式

```
flex-start		子元素排列在主轴的起始端
flex-end		子元素排列在主轴的末端
center			子元素在主轴方向上居中对齐
space-between	子元素在主轴方向上均匀分布，第一个子元素靠起始端，最后一个子元素靠末端
space-around	子元素在主轴方向上均匀分布，每个子元素的两侧都有相等的间距
space-evenly	子元素在主轴方向上均匀分布，每个子元素之间的间距相等，包括第一个子元素和最后一个子元素与容器边界的间距
```

对主轴控制

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .menu {
            border: 1px solid red;
            width: 500px;

            display: flex;
            flex-direction: row; /*主轴=横向*/
            /*justify-content: space-evenly;*/
            justify-content: space-between;

        }
        .menu .item{
            width: 45px;
            height: 50px;
            border: 1px solid green;
        }
    </style>
</head>
<body>
<div class='menu'>
    <div class='item'>北京</div>
    <div class='item'>上海</div>
    <div class='item'>深圳</div>
</div>


</body>
</html>
```

既控制主轴也控制副轴

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .menu {
            border: 1px solid red;
            width: 500px;
            height: 500px;

            display: flex;
            flex-direction: row; /*主轴=横向*/
            /*justify-content: space-evenly;*/
            justify-content: space-between;

            align-items: center;
            /*align-items: flex-start;*/
            /*align-items: flex-end;*/
        }
        .menu .item{
            width: 45px;
            height: 50px;
            border: 1px solid green;
        }
    </style>
</head>
<body>
<div class='menu'>
    <div class='item'>北京</div>
    <div class='item'>上海</div>
    <div class='item'>深圳</div>
</div>


</body>
</html>
```

#### 4.换行

当元素过多时，子元素可能会被压缩或溢出容器，此时就需要元素换行显示

```js
flex-wrap: nowrap; // 默认值，不换行
flex-wrap: wrap; // 换行
flex-wrap: wrap-reverse; // 换行，方向相反，主轴水平，从底向上换行，主轴垂直，从右向左换行
```



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .menu {
            border: 1px solid red;
            width: 500px;
            height: 500px;

            display: flex;
            flex-direction: row; /*主轴=横向*/
            /*justify-content: space-evenly;*/
            justify-content: flex-start;

            /*align-items: center;*/
            align-items: flex-start;
            /*align-items: flex-end;*/

            flex-wrap: wrap;
        }

        .menu .item {
            width: 45px;
            height: 50px;
            border: 1px solid green;
        }
    </style>
</head>
<body>
<div class='menu'>
    <div class='item'>北京</div>
    <div class='item'>上海</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
</div>


</body>
</html>
```

出现问题，换行后第二行元素并没有贴着第一行显示，而是空了很大的位置



#### 5.多行控制

```
控制多行显示时，行与行之间的排列
align-content: center;
```



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .menu {
            border: 1px solid red;
            width: 500px;
            height: 500px;

            display: flex;
            flex-direction: row; /*主轴=横向*/
            /*justify-content: space-evenly;*/
            justify-content: flex-start;

            /*align-items: center;*/
            align-items: flex-start;
            /*align-items: flex-end;*/

            flex-wrap: wrap;
            align-content: center;
        }
        .menu .item{
            width: 45px;
            height: 50px;
            border: 1px solid green;
        }
    </style>
</head>
<body>
<div class='menu'>
    <div class='item'>北京</div>
    <div class='item'>上海</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
</div>


</body>
</html>
```



#### 案例

存在问题：当数量刚好整除时，排列的没有任何问题，三个元素排满一排空间。但如果最后一行只剩一个或两个元素，就会单独占满一行，从而导致与前几行的排布不同

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .menu {
            border: 1px solid red;
            width: 500px;
            height: 500px;

            display: flex;
            flex-direction: row; /*主轴=横向*/


            /*justify-content: space-between;*/
            justify-content: space-around; /*横轴*/

            align-items: flex-start; /*纵轴*/

            flex-wrap: wrap;

            align-content: flex-start; /*多行文本，从顶部开始*/
        }

        .menu .item {
            width: 150px;
            height: 50px;
            border: 1px solid green;
        }
    </style>
</head>
<body>
<div class='menu'>
    <div class='item'>北京</div>
    <div class='item'>上海</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
    <div class='item'>深圳</div>
</div>


</body>
</html>
```



### 元素

#### 顺序

通过`order`值的大小决定同级元素之间的排布顺序

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .menu {
            border: 1px solid red;
            width: 500px;
            height: 500px;

            display: flex;
            flex-direction: row; /*主轴=横向*/


            /*justify-content: space-between;*/
            justify-content: space-around; /*横轴*/

            align-items: flex-start; /*纵轴*/

            flex-wrap: wrap;

            align-content: flex-start; /*多行文本，从顶部开始*/
        }

        .menu .item {
            width: 50px;
            height: 50px;
            border: 1px solid green;
        }
    </style>
</head>
<body>
<div class='menu'>
    <div class='item' style="order: 1">北京</div>
    <div class='item' style="order: 0">上海</div>
    <div class='item' style="order: 2">深圳</div>
</div>


</body>
</html>
```



#### 剩余空间

通过`flex-grow`定义甚于空间的分布

左对齐，仅自身宽度，右侧一大块剩余空间

```html
<div class='menu'>
    <div class='item' style="">北京</div>
    <div class='item' style="">上海</div>
    <div class='item' style="">深圳</div>
</div>
```

北京占自身宽度，上海和深圳按照2：1的比例把北京以外的剩余空间占满

```html
<div class='menu'>
    <div class='item' style="">北京</div>
    <div class='item' style="flex-grow: 2">上海</div>
    <div class='item' style="flex-grow: 1">深圳</div>
</div>
```

北京和深圳占自身宽度，上海将中间的剩余空间全部占满

```html
<div class='menu'>
    <div class='item' style="">北京</div>
    <div class='item' style="flex-grow: 2">上海</div>
    <div class='item' style="">深圳</div>
</div>
```

完整代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .menu {
            border: 1px solid red;
            width: 500px;
            height: 500px;

            display: flex;
            flex-direction: row; /*主轴=横向*/


            /*justify-content: space-between;*/
            justify-content: flex-start; /*横轴*/
        }

        .menu .item {
            width: 50px;
            height: 50px;
            border: 1px solid green;
        }
    </style>
</head>
<body>
<div class='menu'>
    <div class='item' style="">北京</div>
    <div class='item' style="flex-grow: 2">上海</div>
    <div class='item' style="flex-grow: 1">深圳</div>
</div>


</body>
</html>
```



#### 案例

通过计算右边距来解决上一个案例遗留的问题（当最后一行元素个数不是1，也不足以排满整行时，会出现错位）

```html
/*如果最后一个元素，是第3个，右边距=一个位置 + 所有空白位置/3（有三个空白位置）*/
<style>
	.course-list .item:last-child:nth-child(4n - 1) {
    	margin-right: calc(24% + 4% / 3);
	}

	.course-list .item:last-child:nth-child(4n - 2) {
    	margin-right: calc(48% + 8% / 3);
	}
</style>
```

![image-20221113112314914](/post/Vue/Vue3/image-20221113112314914.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .container {
            width: 1100px;
            margin: 0 auto;
        }

        .row1 {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
        }

        .row1 .company {
            width: 210px;
            height: 180px;
            background-color: saddlebrown;
        }

        .row1 .pic {
            width: 266px;
            height: 180px;
            background-color: cadetblue;
        }

        .row2 .title {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
        }

        .row2 .pic-list {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
        }

        .row2 .pic-list .big {
            background-color: aquamarine;
            height: 610px;
            width: 210px;
            margin-right: 20px;
        }

        .row2 .pic-list .right-list {
            background-color: antiquewhite;
            flex-grow: 1;
        }

        .row2 .pic-list .right-list .group {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            flex-wrap: wrap;
        }

        .row2 .pic-list .right-list .phone {
            margin-bottom: 10px;
            border: 1px solid red;
            width: 200px;
            height: 300px;
        }

        .course-list {
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
        }

        .course-list .item {
            width: 24%;
            height: 100px;
            background-color: skyblue;
            margin-top: 15px;
        }

        /*如果最后一个元素，是第3个，右边距=一个位置 + 所有空白位置/3（有三个空白位置）*/
        .course-list .item:last-child:nth-child(4n - 1) {
            margin-right: calc(24% + 4% / 3);
        }

        .course-list .item:last-child:nth-child(4n - 2) {
            margin-right: calc(48% + 8% / 3);
        }
    </style>
</head>
<body>

<div class="container">

    <div class="row1">
        <div class="company"></div>
        <div class="pic"></div>
        <div class="pic"></div>
        <div class="pic"></div>
    </div>

    <div class="row2">
        <div class="title">
            <div>手机</div>
            <div>查看更多</div>
        </div>

        <div class="pic-list">
            <div class="big"></div>
            <div class="right-list">
                <div class="group">
                    <div class="phone"></div>
                    <div class="phone"></div>
                    <div class="phone"></div>
                    <div class="phone"></div>
                </div>
                <div class="group">
                    <div class="phone"></div>
                    <div class="phone"></div>
                    <div class="phone"></div>
                    <div class="phone"></div>
                </div>
            </div>
        </div>
    </div>

    <div class="course-list">
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>
        <div class="item"></div>

    </div>
</div>

</body>
</html>
```

## vue-router

### 安装

- `npm install vue-router --save`
  - 需要手动创建并编写router/index.js文件，配置main.js和App.vue
- `vue add router`
  - 一键完成所有配置

![image-20250208200643749](/post/Vue/Vue3/image-20250208200643749.png)

可以看到，需要配置的地方都自动完成配置了

![image-20250208201525153](/post/Vue/Vue3/image-20250208201525153.png)



### 必备操作

#### 快速上手（案例）

![image-20221113114601213](/post/Vue/Vue3/image-20221113114601213.png)

详细见：**src-1.zip**

`App.vue`

```vue
<template>
  <div>
    <div class="menu">
      <div class="container">
        <nav>
          <router-link to="/">源代码教育</router-link>
          <router-link to="/home">首页</router-link>
          <router-link to="/course">课程</router-link>
          <router-link to="/news">资讯</router-link>
        </nav>
      </div>
    </div>
    <div class="container">
      <router-view/>
    </div>
  </div>
</template>

<style>
  body{
    margin: 0;
  }
  .container {
    width: 980px;
    margin: 0 auto;
  }

  .menu{
    height: 48px;
    line-height: 48px;
    background-color: #499ef3;
  }
  .menu a{
    text-decoration: none;
    margin: 0 10px;
    color: white;
    display: inline-block;
  }
</style>
<script setup lang="ts">
</script>
```

`router/index.js`

```js
import {createRouter, createWebHistory} from 'vue-router'
import HomeView from '../views/HomeView.vue'

const routes = [
    {
        path: '/',
        name: 'Index',
        component: HomeView
    },
    {
        path: '/home',
        name: 'Home',
        component: HomeView
    },
    {
        path: '/course',
        name: 'Course',
        component: () => import('../views/CourseView.vue'),
    },
    {
        path: '/news',
        name: 'News',
        component: () => import('../views/NewsView.vue')
    }
]

const router = createRouter({
    history: createWebHistory(process.env.BASE_URL),
    routes
})

export default router

```



#### URL传值（GET）

![image-20221113121504119](/post/Vue/Vue3/image-20221113121504119.png)

- 可以在App.vue的router-link标签内的to属性中传递参数

```html
//基础写法
<router-link to="/course">课程1</router-link>
//对象写法（根据path匹配路由）
<router-link :to="{path:'/course'}">课程2</router-link>
//对象写法（根据name匹配路由）
<router-link :to="{name:'Course'}">课程3</router-link>
//对象写法，添加参数（path/name）
<router-link :to="{name:'Course',query:{size:21,page:9}}">课程-21-9</router-link>
<router-link :to="{path:'/course',query:{size:22,page:19}}">课程-22-19</router-link>
```

- 在目标组件中接收参数

问题：Vue 的响应式系统是基于对象的属性访问和修改来工作的。当一个响应式对象的属性发生变化时，Vue 会自动触发视图的更新。然而，Vue Router 的 `$route` 对象是一个特殊的响应式对象，它只会在 路由路径（`path`） 或 路由名称（`name`）发生变化时触发组件的重新加载，因此如果在同一个组件内进行跳转（只有传递参数不同），页面默认不会刷新即我从`/course?page=1&size=1`跳转到`/course?page=1&size=2`，页面数据不变。该如何解决？

1. 使用 `watch` 监听 `$route.query` 的变化
2. 通过给 `<router-view>` 添加一个动态的 `key`，使其在路由变化时重新加载组件
3. 使用 `beforeRouteUpdate` 路由守卫

下列示例采用的是路由守卫解决

```js
// 小知识点
导入useRoute并实例化出类对象route，route.query就是存储了所有传递参数的对象
onBeforeRouteUpdate((to, from)中to就是跳转后的router对象，to.query存储了其传递的所有参数
短路特性，设置默认值0：route.query.page || 0
<script setup></script>在里面书写相当于在setup()中写
vue3要实现响应式，需要用ref包裹变量，变量被包裹后变成对象类型，可以通过对象.value改值
/* eslint-disable */让eslint不检查该文件的语法问题
```

```vue
<script setup>
/* eslint-disable */
import {ref} from 'vue'
import {useRoute, onBeforeRouteUpdate} from 'vue-router'

const route = useRoute();

const name = ref("武沛齐");
const page = ref(route.query.page || 0);
const size = ref(route.query.size || 0);

onBeforeRouteUpdate((to, from) => {
  page.value = to.query.page || 0;
  size.value = to.query.size || 0;
})
</script>

<template>
  <div>
    <h1>课程页面</h1>
    <h3>讲师：{{ name }}</h3>
    <div>当前页：{{ page }}</div>
    <div>数量：{{ size }}</div>
  </div>
</template>

<style scoped>

</style>
```

详细见：**src-2.zip**



#### URL动态参数

![image-20221113142551078](/post/Vue/Vue3/image-20221113142551078.png)

上面一种情况是get请求时携带参数（Query String），参数跟在?后面，例如xxx.xxx.com/course?page=2&size=1

除此之外，在路径中也可以携带参数

```vue
<router-link :to="{name:'Detail',params:{id:99},query:{size:22,page:19}}">详细-99-22-19</router-link>
<router-link :to="{name:'Detail',params:{id:100},query:{size:11,page:19}}">详细-100-11-19</router-link>
```

组件内接收

传递的参数对象存储在`route.params`中，同样的也会出现同页面跳转的不刷新问题，同样使用路由守卫手动赋值

```vue
<script setup>
import {useRoute, onBeforeRouteUpdate} from "vue-router";
import {ref} from "vue";

const route = useRoute();
const v1 = ref(route.query);
const v2 = ref(route.params);

onBeforeRouteUpdate((to) => {
  v1.value = to.query;
  v2.value = to.params;
})

</script>

<template>
  <h2>详细</h2>
  <div>
    <div>{{ v1 }}</div>
    <div>{{ v2 }}</div>
  </div>
</template>

<style scoped>

</style>
```

详细见：**src-3.zip**



#### 路由嵌套

小tip：

例如`/pin`是一个一级页面，旗下有`/pin/hot`，`/pin/new`等二级页面，当访问/pin时，应当将而二级页面的内容也渲染到页面中，即自动选择一个二级页面。有两种实现

- 在配置二级路由时，如果没有匹配空，也给他配置一个组件即访问/pin会加载NewView.vue

  - ```js
    {
        path: '',
        component: () => import('../views/NewView.vue'),
    },
    ```

- 跳转

  - 在二级路由处配置跳转

    - ```js
      {
          path: '',
          redirect: {name: "New"},
      },
      ```

  - 一级路由配置时直接指向默认的二级路由

    - 当访问`/pin`时，直接指向`/pin/new`

    - ```vue
      //旧
      <router-link to="/pins">沸点</router-link>
      //新（两种写法都可以）
      <router-link to="/pins/new">沸点</router-link>
      <router-link :to="{name:'New'}">沸点</router-link>
      ```



App.vue

```vue
<router-link to="/">源代码教育</router-link>
<router-link to="/home">首页</router-link>
<router-link to="/course">课程</router-link>
<router-link to="/pins">沸点</router-link>
<router-link to="/news">资讯</router-link>
```

PinsView.vue

```vue
<template>
    <div>
        <div>上面的内容</div>
        <div>
            <h3>左边菜单</h3>
            <router-link :to="{name:'Hot'}">热点</router-link>
            <router-link :to="{name:'New'}">最新</router-link>
            <router-link :to="{name:'Following'}">关注</router-link>
        </div>
        <div>右边的内容</div>
        <div>
            <router-view/>
        </div>
    </div>
</template>

<script>
    export default {
        name: "PinsView"
    }
</script>

<style scoped>

</style>

```

router/index.js

```js
import {createRouter, createWebHistory} from 'vue-router'
import HomeView from '../views/HomeView.vue'

const routes = [
    {
        path: '/',
        name: "Index",
        component: HomeView
    },
    {
        path: '/home',
        name: 'Home',
        component: HomeView
    },
    {
        path: '/pins',
        // name: 'Pins',
        component: () => import('../views/PinsView.vue'),
        children: [
            {
                path: '',
                component: () => import('../views/NewView.vue'),
            },
            {
                path: 'new',
                name: 'New',
                component: () => import('../views/NewView.vue'),
            },
            {
                path: 'hot',
                name: 'Hot',
                component: () => import('../views/HotView.vue'),
            },
            {
                path: 'following',
                name: 'Following',
                component: () => import('../views/FollowingView.vue'),
            }
        ]
    },
    {
        path: '/course',
        name: 'Course',
        component: () => import('../views/CourseView.vue')
    },
    {
        path: '/detail/:id',
        name: 'Detail',
        component: () => import('../views/DetailView.vue')
    },
    {
        path: '/news',
        name: 'News',
        component: () => import('../views/NewsView.vue')
    }
]

const router = createRouter({
    history: createWebHistory(),
    routes
})

export default router

```



#### 编程式导航

想要实现点击跳转到其他页面并加载组件，常见的有两种实现：

一个就是我们前面使用的router-link标签，再一个就是我们的编程式导航

在 Vue2的Router 中，可以通过 `this.$router.push()`、`this.$router.replace()` 和 `this.$router.go()` 等方法实现编程式导航

 `this.$router.push()`与`this.$router.replace()`的语法和效果几乎相同，唯一的区别就是， `this.$router.push()`是在历史记录中push了一个新纪录，当点击回退时可以返回。而`this.$router.replace()`是replace当前记录，点击回退时无法返回原页面

`this.$router.go()` 方法用于在浏览器的历史记录中前进或后退指定的步数。

在Vue3中，没有`this.$router`，需要我们手动导入并实例化一个router对象

详细如下：

传递一个对象，可以有name或者path，传参使用params和query

```vue
<template>
    <div>
        <h1>新闻页面</h1>
        <input type="button" value="跳转" @click="doClick"/>
    </div>
</template>

<script setup>
    import {useRouter} from 'vue-router'

    const router = useRouter();


    function doClick() {
        //跳转到首页 [Course,]
        // router.push({path:"/home"})
        // router.push({name:"Home"})
        // router.push({name: "Course", query: {page: 10, size: 20}})
        router.push({name: "Detail", params: {id: 100}, query: {page: 10, size: 20}})

        // router.replace({path:"/home"})
        // router.replace({name:"Home"})
        // router.replace({name: "Course", query: {page: 10, size: 20}})
        // router.replace({name: "Detail", params: {id: 100}, query: {page: 10, size: 20}})
        // router.replace({name: "Course", query: {page: 10, size: 20}})

    }
</script>

<style scoped>

</style>
```



##### 登录跳转（含顶部）

与其他一级标题相同，只需要编写自己的页面，然后切换时就可以嵌入到router-view中，保留顶部

![image-20221113150609761](/post/Vue/Vue3/image-20221113150609761.png)

`App.vue`

```vue
<nav>
    <router-link to="/">源代码教育</router-link>
    <router-link to="/home">首页</router-link>
    <router-link to="/course">课程</router-link>
    <router-link :to="{name:'New'}">沸点</router-link>
    <router-link to="/news">资讯</router-link>
    <div style="float: right">
    	<router-link :to="{name:'Login'}">登录</router-link>
    </div>
</nav>
```

`index.js`

```js
import {createRouter, createWebHistory} from 'vue-router'
import HomeView from '../views/HomeView.vue'

const routes = [
    {
        path: '/',
        name: "Index",
        component: HomeView
    },
    {
        path: '/home',
        name: 'Home',
        component: HomeView
    },
    {
        path: '/pins',
        // name: 'Pins',
        component: () => import('../views/PinsView.vue'),
        children: [
            {
                path: '',
                component: () => import('../views/NewView.vue'),
            },
            {
                path: 'new',
                name: 'New',
                component: () => import('../views/NewView.vue'),
            },
            {
                path: 'hot',
                name: 'Hot',
                component: () => import('../views/HotView.vue'),
            },
            {
                path: 'following',
                name: 'Following',
                component: () => import('../views/FollowingView.vue'),
            }
        ]
    },
    {
        path: '/course',
        name: 'Course',
        component: () => import('../views/CourseView.vue')
    },
    {
        path: '/detail/:id',
        name: 'Detail',
        component: () => import('../views/DetailView.vue')
    },
    {
        path: '/news',
        name: 'News',
        component: () => import('../views/NewsView.vue')
    },
    {
        path: '/login',
        name: 'Login',
        component: () => import('../views/LoginView.vue')
    }
]

const router = createRouter({
    history: createWebHistory(),
    routes
})

export default router

```

`LoginView.vue`

```vue
<script setup>
import {ref} from "vue";
import {useRouter} from "vue-router";

const router = useRouter();
const username = ref("");
const password = ref("");

function doLogin() {
  if (username.value.length > 0 && password.value.length > 0) {
    console.log("登录成功")
    router.replace({name: "Home"})

  } else {
    console.log("失败")
  }
}
</script>

<template>
  <div style="width: 400px;height: 200px;margin: 100px auto;">
    <input type="text" v-model="username">
    <input type="password" v-model="password">
    <input type="button" value="登录" @click="doLogin">
  </div>
</template>

<style scoped>

</style>
```

详见：src-4.zip



##### 登录跳转（不含顶部）

需要将登录与其他一级路由独立开来，路由结构如下图，这样就可以保证登录界面无顶部，而其他页面都有导航栏

![image-20250209194053844](/post/Vue/Vue3/image-20250209194053844.png)

![image-20221113151724446](/post/Vue/Vue3/image-20221113151724446.png)

![image-20221113151735710](/post/Vue/Vue3/image-20221113151735710.png)

详见：src-5.zip

App.vue

```vue
<template>
  <div>
      <router-view/>
  </div>
</template>

<style>
body {
  margin: 0;
}

.container {
  width: 980px;
  margin: 0 auto;
}


</style>
<script setup lang="ts">
</script>
```

index.js

```js
import {createRouter, createWebHistory} from 'vue-router'
import HomeView from '../views/HomeView.vue'

const routes = [
    {
        path: '/login',
        name: 'Login',
        component: () => import('../views/LoginView.vue')
    },
    {
        path: '/',
        component: HomeView,
        children: [
            {
                path: '',
                redirect: {name: 'Index'},
            },
            {
                path: 'index',
                name: 'Index',
                component: () => import('../views/IndexView.vue')
            },
            {
                path: '/course',
                name: 'Course',
                component: () => import('../views/CourseView.vue')
            },
            {
                path: '/news',
                name: 'News',
                component: () => import('../views/NewsView.vue')
            },
            {
                path: '/pins',
                component: () => import('../views/PinsView.vue'),
                children: [
                    {
                        path: '',
                        redirect: {name: 'New'},
                    },
                    {
                        path: 'new',
                        name: 'New',
                        component: () => import('../views/NewView.vue'),
                    },
                    {
                        path: 'hot',
                        name: 'Hot',
                        component: () => import('../views/HotView.vue'),
                    },
                    {
                        path: 'following',
                        name: 'Following',
                        component: () => import('../views/FollowingView.vue'),
                    }
                ]
            },
        ]
    },
]

const router = createRouter({
    history: createWebHistory(),
    routes
})

export default router

```

HomeView.vue

```vue
<template>
  <div>
    <div class="menu">
      <div class="container">
        <nav>
          <router-link to="/">源代码教育</router-link>
          <router-link to="/index">首页</router-link>
          <router-link to="/course">课程</router-link>
          <router-link to="/pins">沸点</router-link>
          <router-link to="/news">资讯</router-link>
          <div style="float: right">
            <router-link :to="{name:'Login'}">登录</router-link>
          </div>
        </nav>
      </div>
    </div>
    <div class="container">
      <router-view></router-view>

    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'HomeView',

}
</script>

<style>
.menu {
  height: 48px;
  line-height: 48px;
  background-color: #499ef3;
}

.menu a {
  text-decoration: none;
  margin: 0 10px;
  color: white;
  display: inline-block;
}
</style>
```

PinsVIew.vue

```vue
<template>
  <div>
    <div>上面的内容</div>
    <div>
      <h3>左边菜单</h3>
      <router-link :to="{name:'Hot'}">热点</router-link>
      <router-link :to="{name:'New'}">最新</router-link>
      <router-link :to="{name:'Following'}">关注</router-link>
    </div>
    <div>右边的内容</div>
    <div>
      <router-view/>
    </div>
  </div>
</template>

<script>
export default {
  name: "PinsView"
}
</script>

<style scoped>

</style>

```



#### 导航守卫（全局）

![image-20221113154651481](/post/Vue/Vue3/image-20221113154651481.png)

main.js配置全局导航守卫

```js
import {createRouter, createWebHistory} from 'vue-router'
import HomeView from '../views/HomeView.vue'

const routes = [
    {
        path: '/login',
        name: 'Login',
        component: () => import('../views/LoginView.vue'),
    },
    ...
]

const router = createRouter({
    history: createWebHistory(),
    routes
})


router.beforeEach((to, from, next) => {
    // to，即将访问路由对象
    // from，当前正要离开路由
    // next() 继续向后执行，去to的页面
    // next(false) 不跳转，还在当前页面。
    // next("/xxx")  next({name:"xxx"})  next({pat:"/xxx"}) 指定跳转目标
    let token = sessionStorage.getItem("isLogin");

    if (token) {
        // 已登录，可以向目标地址访问
        next();
        return
    }

    // 未登录，判断要访问的是否是登录页面
    if (to.name === "Login") {
        next();
        return;
    }

    // 未登录，指定跳转回登录页面
    next({name: "Login"});

})


export default router

```

LoginView.vue

对于登录成功的用户，给sessionStorage中赋值

```vue
<template>
    <div style="width: 400px;height: 200px;margin: 100px auto;">
        <input type="text" v-model="username">
        <input type="password" v-model="password">
        <input type="button" value="登录" @click="doLogin"/>
    </div>
</template>

<script setup>
    import {useRouter} from 'vue-router'
    import {ref} from 'vue'

    const router = useRouter();

    const username = ref("");
    const password = ref("");

    function doLogin() {
        //...
        if (username.value.length > 0 && password.value.length > 0) {
            console.log("登录成功");
            sessionStorage.setItem("isLogin",true);
            router.replace({name: "Index"})
        } else {
            console.log("登录失败");
        }
    }
</script>

<style scoped>

</style>

```



详见：src-6.zip

## 存储数据

- cookie，超时时间+发送请求自动携带

- sessionStorage，当浏览器关闭时，自动清除

- localStorage，长时间在存储浏览器

  ```
  localStorage.setItem(key,value)
  localStorage.getItem(key)
  localStorage.removeItem(key)
  localStorage.clear()
  ```


常用cookie和localStorage，且localStorage操作比较方便

## vuex

### 安装

```
npm install vue-vuex --save
手动创建文件+配置
```

或

```
vue add vuex （推荐）
```

![image-20250209205551225](/post/Vue/Vue3/image-20250209205551225.png)

### 使用示例



#### 案例（vuex+存储+导航守卫）

梳理：

```
导航守卫-->router/index.js中router.beforeEach方法检测localStorage中是否有token或username，如没有即未登录，还要访问除了登录页面以外的网页，直接跳转回登录
登录：commit触发login方法，将登录信息传递给vuex，vuex负责将信息存储到内存中state的username和token，以及localStorage中（当页面刷新时重新加载state中的username和token）
homeView.vue：当state.username有数据时，登录按钮会变成用户的username且点击触发注销功能
注销：commit触发logout方法并且跳转回登录页面

state和localStorage的操作都放在vuex中，外部通过commit间接操作state，方便维护
```

router/index.js

```js
import {createRouter, createWebHistory} from 'vue-router'
import HomeView from '../views/HomeView.vue'

const routes = [
    {
        path: '/login',
        name: 'Login',
        component: () => import('../views/LoginView.vue')
    },
    {
        path: '/',
        component: HomeView,
        children: [
            {
                path: '',
                redirect: {name: 'Index'},
            },
            {
                path: 'index',
                name: 'Index',
                component: () => import('../views/IndexView.vue')
            },
            {
                path: '/course',
                name: 'Course',
                component: () => import('../views/CourseView.vue')
            },
            {
                path: '/news',
                name: 'News',
                component: () => import('../views/NewsView.vue')
            },
            {
                path: '/pins',
                component: () => import('../views/PinsView.vue'),
                children: [
                    {
                        path: '',
                        redirect: {name: 'New'},
                    },
                    {
                        path: 'new',
                        name: 'New',
                        component: () => import('../views/NewView.vue'),
                    },
                    {
                        path: 'hot',
                        name: 'Hot',
                        component: () => import('../views/HotView.vue'),
                    },
                    {
                        path: 'following',
                        name: 'Following',
                        component: () => import('../views/FollowingView.vue'),
                    }
                ]
            },
        ]
    },
]

const router = createRouter({
    history: createWebHistory(),
    routes
})

router.beforeEach((to, from, next) => {
    // to，即将访问路由对象
    // from，当前正要离开路由
    // next() 继续向后执行，去to的页面
    // next(false) 不跳转，还在当前页面。
    // next("/xxx")  next({name:"xxx"})  next({pat:"/xxx"}) 指定跳转目标
    let token = localStorage.getItem("token");

    if (token) {
        // 已登录，可以向目标地址访问
        next();
        return
    }

    // 未登录，判断要访问的是否是登录页面
    if (to.name === "Login") {
        next();
        return;
    }

    // 未登录，指定跳转回登录页面
    next({name: "Login"});

})

export default router

```

store/index.js

```js
import {createStore} from 'vuex'

export default createStore({
    state: {
        username: localStorage.getItem('username'),
        token: localStorage.getItem('token'),
    },
    getters: {},
    mutations: {
        login(state, {username, token}) {
            state.username = username
            state.token = token
            //保存在localStorage中
            localStorage.setItem('username', username)
            localStorage.setItem('token', token)
        },
        logout(state) {
            state.username = ''
            state.token = ''
            //清除localStorage
            localStorage.removeItem('username')
            localStorage.removeItem('token')
        }
    },
    actions: {},
    modules: {}
})

```

HomeView.vue

```vue
<template>
  <div>
    <div class="menu">
      <div class="container">
        <nav>
          <router-link to="/">源代码教育</router-link>
          <router-link to="/index">首页</router-link>
          <router-link to="/course">课程</router-link>
          <router-link to="/pins">沸点</router-link>
          <router-link to="/news">资讯</router-link>
          <div style="float: right">
            <a v-if="name" @click="doLogout">{{ name }}</a>
            <router-link v-else :to="{name:'Login'}">登录</router-link>
          </div>
        </nav>
      </div>
    </div>
    <div class="container">
      <router-view></router-view>

    </div>
  </div>
</template>

<script setup>
import {ref} from "vue";
import {useStore} from "vuex";
import {useRouter} from "vue-router";

const router = useRouter();
const store = useStore();
const name = ref(store.state.username)

function doLogout() {
  //localStorage清空 + state值清空 + 跳转登录
  store.commit("logout")
  router.push({name: 'Login'});
}
</script>

<style>
.menu {
  height: 48px;
  line-height: 48px;
  background-color: #499ef3;
}

.menu a {
  text-decoration: none;
  margin: 0 10px;
  color: white;
  display: inline-block;
}
</style>
```

LoginView.vue

```vue
<script setup>
import {ref} from "vue";
import {useRouter} from "vue-router";
import {useStore} from "vuex";

const router = useRouter();
const store = useStore();

const username = ref("");
const password = ref("");

function doLogin() {
  if (username.value.length > 0 && password.value.length > 0) {
    console.log("登录成功");
    //调用login方法
    const response = {
      username: username.value,
      token: "ed5cf238-d8ce-49f9-a1cf-cef55d689a2b",
      id: 1000
    }
    store.commit("login", response)
    router.replace({path: "/index"})
    console.log("跳转成功")

  } else {
    console.log("失败")
  }

}
</script>

<template>
  <div style="width: 400px;height: 200px;margin: 100px auto;">
    <input type="text" v-model="username">
    <input type="password" v-model="password">
    <input type="button" value="登录" @click="doLogin">
  </div>
</template>

<style scoped>

</style>
```

详见：src10.zip



#### 案例（动态购物车）

实现点击按钮加入购物车，页面右上角显示购物车商品数量也实时+1

NewsView.vue

```vue
<script setup>
import {useStore} from "vuex";

const store = useStore();

function addCar() {
  //当前购物车数量+1
  store.commit("addCar");
}
</script>

<template>
  <h1>新闻页面</h1>
  <input type="button" value="加入购物车" @click="addCar">

</template>

<style scoped>

</style>
```

store/index.js

```js
import {createStore} from 'vuex'

export default createStore({
    state: {
        username: localStorage.getItem('username'),
        token: localStorage.getItem('token'),
        car: 0,
    },
    getters: {},
    mutations: {
        login(state, {username, token}) {
            state.username = username
            state.token = token
            //保存在localStorage中
            localStorage.setItem('username', username)
            localStorage.setItem('token', token)
        },
        logout(state) {
            state.username = ''
            state.token = ''
            //清除localStorage
            localStorage.removeItem('username')
            localStorage.removeItem('token')
        },
        addCar(state) {
            state.car += 1
            console.log(state.car)
        }
    },
    actions: {},
    modules: {}
})

```

HomeView.vue

```vue
<template>
  <div>
    <div class="menu">
      <div class="container">
        <nav>
          <router-link to="/">源代码教育</router-link>
          <router-link to="/index">首页</router-link>
          <router-link to="/course">课程</router-link>
          <router-link to="/pins">沸点</router-link>
          <router-link to="/news">资讯</router-link>
          <div style="float: right">
            <span>购物车有{{ carNum }}件商品</span>
            <a v-if="name" @click="doLogout">{{ name }}</a>
            <router-link v-else :to="{name:'Login'}">登录</router-link>
          </div>
        </nav>
      </div>
    </div>
    <div class="container">
      <router-view></router-view>

    </div>
  </div>
</template>

<script setup>
import {ref} from "vue";
import {useStore} from "vuex";
import {useRouter} from "vue-router";

const router = useRouter();
const store = useStore();
const name = ref(store.state.username)
const carNum = ref(store.state.car)

function doLogout() {
  //localStorage清空 + state值清空 + 跳转登录
  store.commit("logout")
  router.push({name: 'Login'});
}
</script>

<style>
.menu {
  height: 48px;
  line-height: 48px;
  background-color: #499ef3;
}

.menu a {
  text-decoration: none;
  margin: 0 10px;
  color: white;
  display: inline-block;
}
</style>
```

但是运行发现，carNum数量确实加了，但是页面并没有变化（因为浏览器没有因此刷新）

这就需要我们用到计算属性（购物车数量不同于用户名，登陆上了就不容易发生变化，相反，他是实时变化的，因此不适合使用普通的const对他进行一次赋值就放任不管，而是要实时检测数据，一旦发生变化，立刻更新）

```js
const name = ref(store.state.username)
const carNum = ref(store.state.car)
```

同时，需要将数据备份到本地，当刷新后，从本地读取到内存中，实现记忆

store/index.js

```js
import {createStore} from 'vuex'

export default createStore({
    state: {
        username: localStorage.getItem('username'),
        token: localStorage.getItem('token'),
        car: localStorage.getItem('carNum') || 0,
    },
    getters: {},
    mutations: {
        login(state, {username, token}) {
            state.username = username
            state.token = token
            //保存在localStorage中
            localStorage.setItem('username', username)
            localStorage.setItem('token', token)
        },
        logout(state) {
            state.username = ''
            state.token = ''
            //清除localStorage
            localStorage.removeItem('username')
            localStorage.removeItem('token')
        },
        addCar(state) {
            state.car = parseInt(state.car) + 1
            localStorage.setItem('carNum', state.car)
        }
    },
    actions: {},
    modules: {}
})

```

HomeView.vue

```vue
<template>
  <div>
    <div class="menu">
      <div class="container">
        <nav>
          <router-link to="/">源代码教育</router-link>
          <router-link to="/index">首页</router-link>
          <router-link to="/course">课程</router-link>
          <router-link to="/pins">沸点</router-link>
          <router-link to="/news">资讯</router-link>
          <div style="float: right">
            <span>购物车有{{ carNum }}件商品</span>
            <a v-if="name" @click="doLogout">{{ name }}</a>
            <router-link v-else :to="{name:'Login'}">登录</router-link>
          </div>
        </nav>
      </div>
    </div>
    <div class="container">
      <router-view></router-view>

    </div>
  </div>
</template>

<script setup>
import {ref, computed} from "vue";
import {useStore} from "vuex";
import {useRouter} from "vue-router";

const router = useRouter();
const store = useStore();
const name = ref(store.state.username)
const carNum = computed(() => store.state.car)

function doLogout() {
  //localStorage清空 + state值清空 + 跳转登录
  store.commit("logout")
  router.push({name: 'Login'});
}
</script>

<style>
.menu {
  height: 48px;
  line-height: 48px;
  background-color: #499ef3;
}

.menu a {
  text-decoration: none;
  margin: 0 10px;
  color: white;
  display: inline-block;
}
</style>
```



详见：src-10.zip



## vue3-cookies

实现将数据在cookie中进行存取。



### 安装

```
npm install vue3-cookies --save
```

main.js配置

```javascript
import {createApp} from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import VueCookies from 'vue3-cookies'

createApp(App).use(store).use(router).use(VueCookies).mount('#app')
```

使用

```vue
import {useCookies} from 'vue3-cookies'

const {cookies} = useCookies();

cookies.set("ts", "123123", "10s")
```

https://github.com/KanHarI/vue3-cookies



### 使用示例

在上面的案例中，将username和token保存到cookie中，设置生效时间。

对于常用的cookie操作，我们可以将其写到一个文件中封装为一个个函数，需要使用时引入即可

`/src/plugins/cookie.js`

```js
import {useCookies} from 'vue3-cookies'

const {cookies} = useCookies();

export const getToken = () => {
    return cookies.get('token') || ""
}
export const getUserName = () => {
    return cookies.get('username') || ""
}
export const setUserToken = (user, token) => {
    cookies.set('username', user, "10s")
    cookies.set('token', token, "10s")
}
export const clearUserToken = () => {
    cookies.remove('username')
    cookies.remove('token')
}
```

使用时只需要从cookie.js中导入需要使用的函数，然后直接调用即可

修改后的/store/index.js

```js
import {createStore} from 'vuex'
import {getUserName, getToken, setUserToken, clearUserToken} from "@/plugins/cookie";

export default createStore({
    state: {
        // username: localStorage.getItem('username'),
        // token: localStorage.getItem('token'),
        username: getUserName(),
        token: getToken(),
        car: localStorage.getItem('carNum') || 0,
    },
    getters: {},
    mutations: {
        login(state, {username, token}) {
            state.username = username
            state.token = token
            //保存在localStorage中
            // localStorage.setItem('username', username)
            // localStorage.setItem('token', token)
            setUserToken(username, token)
        },
        logout(state) {
            state.username = ''
            state.token = ''
            //清除localStorage
            // localStorage.removeItem('username')
            // localStorage.removeItem('token')
            clearUserToken()
        },
        addCar(state) {
            state.car = parseInt(state.car) + 1
            localStorage.setItem('carNum', state.car)
        }
    },
    actions: {},
    modules: {}
})

```

同样的，router/index.js中导航守卫的操作也需要更新

```js
import {getToken} from "@/plugins/cookie";

router.beforeEach((to, from, next) => {

    let token = getToken();

    if (token) {
        // 已登录，可以向目标地址访问
        next();
        return
    }

    // 未登录，判断要访问的是否是登录页面
    if (to.name === "Login") {
        next();
        return;
    }

    // 未登录，指定跳转回登录页面
    next({name: "Login"});

})
```

详见：src11.zip

## axios



### 安装

```
npm install axios --save
手动配置
```

或

```
vue add axios
```

因为我们需要在它本身的基础上进行拓展，因此直接选取npm安装然后手动配置

主要配置文件时/src/plugins/axios.js

### 使用示例



#### 快速发送

axios.js

```js
/* eslint-disable */

import axios from "axios";

axios.defaults.baseURL = 'https://api.luffycity.com/api/';
// axios.defaults.headers.common['Authorization'] = getToken();
// axios.defaults.headers.post['Content-Type'] = 'application/json';


let config = {
    // baseURL: process.env.baseURL || process.env.apiUrl || ""
    // timeout: 60 * 1000, // Timeout
    // withCredentials: true, // Check cross-site Access-Control
};

const _axios = axios.create(config);



export default _axios;

```

LoginView.vue

```vue
<script setup>
import {ref} from "vue";
import {useRouter} from "vue-router";
import {useStore} from "vuex";
import _axios from "@/plugins/axios";

const router = useRouter();
const store = useStore();

const username = ref("");
const password = ref("");

function doLogin() {
  //发送网路请求
  _axios.post("/v1/auth/password/login/?loginWay=password", {
    username: "alex",
    password: "dsb"
  }).then((res) => {
    // {"code":-1,"msg":"校验错误","data":{"global_error":["密码错误"]}}
    console.log("成功", res.data);
  }).catch((error) => {
    console.log("失败", error);
  })


  if (username.value.length > 0 && password.value.length > 0) {
    console.log("登录成功");
    //调用login方法
    const response = {
      username: username.value,
      token: "ed5cf238-d8ce-49f9-a1cf-cef55d689a2b",
      id: 1000
    }
    store.commit("login", response)
    router.replace({path: "/index"})
    console.log("跳转成功")

  } else {
    console.log("失败")
  }

}
</script>

<template>
  <div style="width: 400px;height: 200px;margin: 100px auto;">
    <input type="text" v-model="username">
    <input type="password" v-model="password">
    <input type="button" value="登录" @click="doLogin">
  </div>
</template>

<style scoped>

</style>
```

详见：src12.zip



#### 拦截器

发送每个请求执行操作，每个请求返回的执行操作。

```js
/* eslint-disable */

import axios from "axios";

import {useRouter} from "vue-router";
import {useStore} from "vuex";
import {getToken} from "@/plugins/cookie";

const router = useRouter();
const store = useStore();
axios.defaults.baseURL = 'https://api.luffycity.com/api/';
// axios.defaults.headers.common['Authorization'] = getToken();
// axios.defaults.headers.post['Content-Type'] = 'application/json';


let config = {
    // baseURL: process.env.baseURL || process.env.apiUrl || ""
    // timeout: 60 * 1000, // Timeout
    // withCredentials: true, // Check cross-site Access-Control
};

const _axios = axios.create(config);

_axios.interceptors.request.use(
    function (config) {
        // Do something before request is sent
        // console.log("请求前执行");
        const token = getToken();
        if (token) {
            config.headers['token'] = token;
        }
        return config;
    }
);


// 浏览器上有Token，但是Token在后端已经失效
_axios.interceptors.response.use(
    function (response) {
        // Do something with response data
        // 请求成功 200成功（登录失效了）{code:-1,msg:"登录失效"}   {code:0,msg:data}   {code:1000,msg:"认证失败"}
        if (response.data.code === 1000) {
            //认证失败，token过期，登录失败 -> 登录页面
            store.commit("logout");
            router.replace({name: "Login"});
            return Promise.reject();
        }

        return response;
    },
    function (error) {
        // Do something with response error
        // 请求失败自动执行此处的代码，返回的状态码：500（认证401）
        if (error.response.status === 401) {
            store.commit("logout");
            router.replace({name: "Login"});
        }
        return Promise.reject(error);
    }
);


export default _axios;

```

对于请求失败与否，从请求本身的角度来讲，200是请求成功，其他状态码比如401等等就属于请求失败，会触发响应拦截器的`function (error)`。但这种请求码有局限，类型太少了，因此企业会人为规定一些状态码，就不采用200这种形式

而是在response中添加一个status参数来显示状态，例如0表示成功，-1表示失败等。这种就需要在响应拦截器的`function (response)`中人为做进一步区分。

请求拦截器中可以用于在请求中携带token等等操作

总之，有了拦截器就可以对发送的请求和服务器的响应高度定制，灵活控制



详见：src-12.zip



## 后端API



### 跨域问题

当前浏览器访问地址：http://localhost:8080/news

    点击发送网络请求：https://api.luffycity.com/api/xxxx/xxxx/



本质上想要处理跨域，添加一些响应头即可。

如果发送的网址，请求方式或者包含了不被允许的请求头，预检不通过，浏览器不会发起真实的请求

```python
from django.shortcuts import render
from django.http import JsonResponse


def user_list(request):
    info = {"code": 0, 'data': "success"}
    response = JsonResponse(info)

    # 响应头
    print(request.method)

    # 任意网址
    response["Access-Control-Allow-Origin"] = "*"
    # 任意的请求方式
    response["Access-Control-Allow-Methods"] = "*"  # "PUT,DELETE,GET,POST"
    # 允许任意的请求头
    response["Access-Control-Allow-Headers"] = "*"

    return response
```

注意：测试时一定要移除csrf认证。



### 现象（2个请求）

跨域时发送的是：

- 简单请求：1个请求
- 复杂请求：2个请求
  - OPTIONS请求，预检
  - 真正的请求

```
条件：
  ``1``、请求方式：HEAD、GET、POST
  ``2``、请求头信息：
    ``Accept
    ``Accept``-``Language
    ``Content``-``Language
    ``Last``-``Event``-``ID
    ``Content``-``Type` `对应的值是以下三个中的任意一个
                ``application``/``x``-``www``-``form``-``urlencoded
                ``multipart``/``form``-``data
                ``text``/``plain
```

```h
注意：同时满足以上两个条件时，则是简单请求，否则为复杂请求
```



如果非要避免这种情况，那就别跨域。

- 主域名+API域名（有跨域）

  ```
  https://www.luffycity.com/free-course
  https://api.luffycity.com/api/v1/course/category/actual/?courseType=actual
  ```

- 主域名+API域名（无跨域，但要防止与前端使用的地址冲突）

  ```
  https://www.luffycity.com/free-course
  https://www.luffycity.com/api/...
  ```

  

### 最后跨域

```python
from django.shortcuts import render
from django.http import JsonResponse


def user_list(request):
    info = {"code": 0, 'data': "success"}
    response = JsonResponse(info)

    # 响应头
    print(request.method)

    # 任意网址
    response["Access-Control-Allow-Origin"] = "*"
    # 任意的请求方式
    response["Access-Control-Allow-Methods"] = "*"  # "PUT,DELETE,GET,POST"
    # 允许任意的请求头
    response["Access-Control-Allow-Headers"] = "*"

    return response
```

写在中间件的process_response中

在django的settings.py中将中间件用上即可

```python
from django.utils.deprecation import MiddlewareMixin


class CorsMiddleware(MiddlewareMixin):
    def process_response(self, request, response):
        # 任意网址
        response["Access-Control-Allow-Origin"] = "*"
        # 任意的请求方式
        response["Access-Control-Allow-Methods"] = "*"   # "PUT,DELETE,GET,POST"
        # 允许任意的请求头
        response["Access-Control-Allow-Headers"] = "*"
        return response
```

