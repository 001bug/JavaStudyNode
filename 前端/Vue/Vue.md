# Vue2
## Vue的概述以及思想
**Vue的简介**
Vue全名为Vue.js , 是一个用于构建用户界面的渐进式JavaScript框架 . 它的核心是让程序员专注于视图层, 适合用于开发单页面应用

**Vue中MVVM思想**
1.M: 即Model模型 , 包括数据和一些基本操作 , 与业务逻辑和数据相关的内容. 可以是服务端的数据 , 前端本地存储的数据 , 也可以是用户输入的数据. 数据模型不予视图交互
2.V: 即View视图 , 是用户界面 , 页面渲染的结果 , 视图通常是由模版(HTML)定义的. 当数据更新时 , 视图会自动更新 , 无需开发者手动操作DOM
3.VM: View-Model, 模型与视图间的双向操作(无需开发人员干涉) . 它是Vue.js中的一个对象 . 
![](assest/Pasted%20image%2020240913111907.png)
在MVVM之前，开发人员从后端获取需要的数据模型，然后要通过DOM 操作Model渲染到View 中。而后当用户操作视图，我们还需要通过DOM获取View 中的数据，然后同步到Model 中。而MVVM中的VM 要做的事情就是把DOM 操作完全封装起来，开发人员不用再关心Model 和View 之间是如何互相影响的.
## 快速入门
1. 创建新文件夹 , 然后直接拖到idea工具 , 使用idea打开
2. 将下载好的vue.js拷贝到上一步创建的文件夹中
3. 创建html文件
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>vue快速入门</title>  
</head>  
<body>  
<div id="app">  
    <h1>欢迎你{{message}}-{{name}}</h1>//插值表达式  
</div>  
<script src="vue.js"></script>  
<script>  
    let vm=new Vue({//vue实例 
        el:"#app",  //挂载到id=app的div重 
        data:{  //data{}表示数据池(model有了数据),有很多数据是以k-v形式设置(根据业务需要来设置)
            message:"Hello-Vue",  
            name:"你好Vue , 遇见你很高兴"  
        }  
    });  
    console.log("vm=",vm);  
    console.log(vm._data.message);  
    console.log(vm._data.name);  
    console.log(vm.name);  
    console.log(vm.message);  
</script>  
</body>  
</html>
```
对模版的解释: 
div元素不是必须的 , 也可以是其他元素 , 比如span,但是约定都是将vue实例挂载到div中 , 因为div更适合做布局 , id也不是必须为app, 是程序员指定, 一般我们就使用app
其中`{{message}}`是插值表达式 , `message`就是从model的data数据池来设置的. 在底层 , 当代码执行时 , 会到`data{}`数据池中区匹配数据 , 如果匹配的上 , 就进行替换 , 没有匹配上 , 就输出空
## 指令
## 数据绑定
### 数据绑定的概念以及验证
**数据绑定的概念**
将**vue实例中**的数据和视图(即HTML元素)进行关联, 使得数据的变化自动反映到视图上 , 视图的变化也自动更新到数据中.

**数据绑定的主要形式**
1.差值绑定
* 定义: 将Vue实例的数据绑定到**HTML元素的文本内容中**
* 语法: 使用双花括号`{{}}`
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>vue快速入门</title>  
</head>  
<body>  
<div id="app">  
    <h1>欢迎你{{message}}-{{name}}</h1>  
</div>  
<script src="vue.js"></script>  
<script>  
    let vm=new Vue({  
        el:"#app",  
        data:{  
            message:"Hello-Vue",  
            name:"你好Vue , 遇见你很高兴"  
        }  
    });  
    console.log("vm=",vm);  
    console.log(vm._data.message);  
    console.log(vm._data.name);  
    console.log(vm.name);  
    console.log(vm.message);  
</script>  
</body>  
</html>
```
* 效果当`message`数据变化时 , `<h1>`标签中的内容也会跟着变化

2.属性绑定
* 定义: 将Vue实例的数据绑定到**HTML元素的属性**中. 注意不是CSS样式(比如img标签中src是属性 , `<p>`标签中 , style是CSS样式而不是属性)
* 语法; 使用`v-bind`指令或者缩写成`:`
```html
<div id="app">
    <img v-bind:src="imageSrc" alt="Vue Logo"> <!-- 绑定 imageSrc 数据到 img 标签的 src 属性 -->
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            imageSrc: 'https://vuejs.org/images/logo.png'
        }
    });
</script>
```

3.指令绑定
* 定义: 使用Vue指令 (如`v-if`,`v-for`,`v-model`)将数据绑定到**HTML元素的属性**或者**行为**中
```html
<div id="app">
    <p v-if="isVisible">This is visible</p> <!-- 条件渲染：当 isVisible 为 true 时显示 -->
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            isVisible: true
        }
    });
</script>
```

**数据绑定分析**
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>vue快速入门</title>  
</head>  
<body>  
<div id="app">  
    <h1>欢迎你{{message}}-{{name}}</h1>  
</div>  
<script src="vue.js"></script>  
<script>  
    let vm=new Vue({  
        el:"#app",  
        data:{  
            message:"Hello-Vue",  
            name:"你好Vue , 遇见你很高兴"  
        }  
    });  
    console.log("vm=",vm);  
    console.log(vm._data.message);  
    console.log(vm._data.name);  
    console.log(vm.name);  
    console.log(vm.message);  
</script>  
</body>  
</html>
```
首先打开f12 , 然后打开控制台 , 输入vue实例 , 查看对象属性
![](assest/Pasted%20image%2020240914083129.png)
也可以通过控制台, 改变数据模型的值,从而达到更新页面的效果, 从中我们可以体会到

**Vue的使用细节**
1.注意代码顺序 , 要求div在前, script在后 , 否则无法绑定数据
2.从案例可以体会声明式渲染 : Vue.js采用简洁的模版语法来声明式地将数据渲染进行dom操作的系统, 做到数据和显示分离
3.Vue没有繁琐的DOM操作 , 如果使用JQuery , 我们需要先找到div节点 , 获取到DOM对象 , 然后进行节点操作 .
### 数据双向绑定
**1.双向绑定指的是**
* 视图更新数据: 当用户在视图(如输入框)中输入内容时 , 模型数据会自动更新.
* 数据更新视图: 当模型数据发生变化时, 视图也会自动反映出新的数据
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>double_blinding</title>  
</head>  
<body>  
<div id="app">  
    <input v-model="message" placeholder="请你输入内容">  
    <p>你输入的内容是:{{message}}</p>  
</div>  
<script src="../vue.js"></script>  
<script>  
    new Vue({  
        el:'#app',  
        data:{  
            message:''  
        }  
    });  
</script>  
</body>  
</html>
```
**解析**
1.v-bind是数据单向绑定 , 数据池绑定的数据变化 , 会影响view
2.这里v-model="message"是双向绑定, data数据池绑定的变化会影响view, view关联的元素值会发生变化 , 也会影响到data数据池的数据
## 事件绑定
### 事件绑定的基本介绍和使用
**基本介绍**
[事件](事件)绑定机制通过指令`v-on`(或简写@)实现. `v-on`允许我们监听DOM事件, 并在事件触发时执行指定的**javascript方法**. 提高了多种高级功能 , 如时间修饰符

**基本事件绑定**
* Vue.js提供了直观的事件绑定方法. 可以通过`v-on`绑定任何`DOM`事件.
```html
<!DOCTYPE html>  
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">  
<head>  
    <meta charset="UTF-8">  
    <title>try</title>  
</head>  
<body>  
<div id="app">  
    <button v-on:click="sayHi">打招呼</button>  
    <script src="../vue.js"></script>  
    <script>  
        new Vue({  
            el:'#app',  
            methods:{  
                sayHi(){  
                    alert("hello , wanderer");  
                }  
            }  
        })  
    </script>  
</div>  
</body>  
</html>
```
数据池里面的指定方法一定要对应

**事件对象**
* 当事件被触发时, Vue会**自动传递原生的DOM事件对象作为参数**. 如果你想使用[事件对象](事件对象) , 可以显式地将其作为方法的参数
```html
<div id="app">
  <button @click="handleClick($event)">点击我</button>
</div>
<script>
  new Vue({
    el: '#app',
    methods: {
      handleClick(event) {
        console.log(event); // 输出事件对象
      }
    }
  });
</script>
```
这个中`handleClick($event)`的`($event)`可以**去掉** , 因为event是事件对象 , 在Vue中会**自动传递原生的DOM事件**对象作为参数
### 事件修饰符
#### 事件修饰符的概念
Vue.js 提供了多种**事件修饰符**，可以简化事件处理逻辑。修饰符是以点 `.` 开头的特殊指令，紧跟在事件名之后，用于修改事件的默认行为，比如阻止事件冒泡、默认行为、单次事件等。修饰符(Modifiers) 是以(.)指明的后缀，指出某个指令以特殊方式绑定。例如，.prevent 修饰符告诉v-on 指令对于触发的事件调用event.preventDefault()即阻止事件原本的默认行为
#### 常用的事件修饰符
**1.`.stop` --- 阻止[[事件冒泡]]**
当一个元素的点击事件触发后 , 默认会冒泡到父级元素. 使用`.stop`可以阻止事件冒泡

**2.`.prevent`-- 阻止默认行为**
常用于阻止表单提交 , 链接跳转等默认行为

**3.`.capture` --- 使用事件捕获模式**
默认情况下, 事件会在冒泡阶段触发 , `.capture`修饰符强制使用事件捕获模式 , 即从父元素向目标元素传递事件(元素自身触发的事件先在此处处理，然后才交由内部元素进行处理)

4.**`.self`--- 只当事件是从当前元素触发时才响应**
该修饰符会确保只有点击当前元素本身时才触发事件

**5.`.once`事件将只会触发一次**
`once` 修饰符确保事件处理器只执行一次，随后自动移除该**事件监听器。**

**6.`.passive` -- 提高滚动事件的性能**
`.passive`修饰符用于告诉浏览器不需要调用`preventDefault()`, 以提升性能, 尤其是在滚动事件中
**应用实例**
```html
<!DOCTYPE html>  
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">  
<head>  
    <meta charset="UTF-8">  
    <title>try</title>  
</head>  
<body>  
<div id="app">  
    <form action="www.baidu.com" @submit.prevent="onMySubmit">  
        妖怪名:<input type="text" v-model="monster.name"><br/>  
        <button type="submit">注册</button>  
    </form>  
    <h1>服务端返回的数据{{count}}</h1>  
</div>  
<script src="../vue.js"></script>  
<script>  
    let vm=new Vue({  
        el:'#app',  
        data:{  
            monster:{  
            },  
            count:0  
        },  
        methods:{  
            onMySubmit(){  
                if(this.monster.name){  
                    console.log(this.monster.name);  
                    this.count=666;  
                }  
                else{  
                    console.log("请输入名字");  
                }  
            }  
        }  
    })  
</script>  
</body>  
</html>
```
## 条件渲染
**概念**
在Vue.js中 , **条件渲染**是指根据某些条件来决定是否渲染某个元素或组件. Vue提供了几个指令来实现条件渲染. 最常用的是`v-if`,`v-else-if`和`v-else`. 这些指令可以控制HTML元素是否在DOM中展示,通俗点说 , 是否在浏览器上展示

**选择性的条件渲染**
- **`v-if`**：如果表达式的值为 `true`，则渲染元素；否则不渲染。
- **`v-else-if`**：表示“否则如果”，用在 `v-if` 后面，当 `v-if` 的条件为 `false` 时，如果 `v-else-if` 的条件为 `true`，渲染该元素。
- **`v-else`**：表示在 `v-if` 和 `v-else-if` 都为 `false` 时渲染这个块。

**选择展示的条件渲染**
`v-show` 只是简单地通过 CSS `display` 属性来隐藏元素，而不会从 DOM 中移除。

```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>condition1</title>  
</head>  
<body>  
<div id="app">  
  <input v-model="score" placeholder="请输入成绩">  
  <p v-if="score>=90">你很优秀</p>  
  <p v-else-if="score>=60">你通过了考试</p>  
  <p v-else>你没有通过开始,请继续努力</p>  
  <p v-show="score<60">你的分数是{{score}}</p>  
  <button @click="changeScore">改变分数</button>  
</div>  
<script src="../vue.js"></script>  
<script>  
  new Vue({  
    el:'#app',  
    data:{  
      score: 75  
    },  
    methods:{  
      changeScore(){  
        this.score=Math.floor(Math.random()*101);  
      }  
    }  
  });  
</script>  
</body>  
</html>
```
## 列表渲染
**概念**
在Vue.js中 , 列表渲染是指使用`v-for`指令根据一个数组或对象来循环渲染一组元素 . 它允许开发者遍历一个数组或对象 , 并根据数据的每一个元素动态生成DOM元素
`v-for`指令用于循环遍历数组或对象 , 并为每个项生成对应的HTML元素 . 通常与`key`结合使用, 以便Vue可以追踪每个元素. 优化渲染性能

**1.数组的列表渲染**
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>repeat</title>  
</head>  
<body>  
<div id="app">  
    <ul>  
        <li v-for="item in items" :key="item.id">{{item.text}}</li>  
    </ul>  
</div>  
<script src="../vue.js"></script>  
<script>  
    new Vue({  
        el:'#app',  
        data:{  
            items:[  
                {id:1,text:'小白'},  
                {id:2,text:'小黑'},  
                {id:3,text:'小黄'}  
            ]  
        }  
    });  
</script>  
</body>  
</html>
```
解析:
- `v-for="item in items"`：`v-for` 循环遍历 `items` 数组中的每一个元素。(循环的主体,`a[n]`)item就是`a[i]`
- `:key="item.id"`：`key` 是为每个循环项生成一个唯一的标识符，帮助 Vue 高效更新和重绘元素。key就是`i`

**对象的列表渲染**
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>object_binding</title>  
</head>  
<body>  
<div id="app">  
    <ul>  
        <li v-for="(key,value) in object">{{key}}:{{value}}</li>  
    </ul>  
</div>  
<script src="../vue.js"></script>  
<script>  
    new Vue({  
        el:'#app',  
        data:{  
            object:{  
                name:'tom',  
                age:25,  
                city:'New York'  
            }  
        }  
    });  
</script>  
</body>  
</html>
```
`key,value`是当前变量 , object是循环对象 , 括号里面可以填两种, `(value,key)`对象遍历 , `(item,index)`
## 组件化编程
**组件化的基本介绍**
组件是Vue.js最强大的功能之一(可以**提高复用性**), 组件本质也是一个Vue实例, 也包括: data, method, 生命周期函数等.组件化编程是一种将应用程序分解为独立、可重用的部分（组件）的开发方法。
**全局组件**
全局组件可以在整个Vue应用中任何地方使用
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Newway_component</title>  
</head>  
<body>  
<div id="app">  
    <counter></counter>  
    <counter></counter>  
</div>  
<script src="../vue.js"></script>  
<script>  
    Vue.component("counter",{  
        template:`<button v-on:click="click()">点击次数={{count}}次</button>`,  
        data(){  
            return{  
                count:10  
            }  
        },  
        methods:{  
            click(){  
                this.count++;  
            }  
        }  
    });  
    new Vue({  
        el:'#app'  
    });  
</script>  
</body>  
</html>
```
**局部组件**
局部组件仅在定义它的**Vue实例**中可用
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>part</title>  
</head>  
<body>  
<div id="app">  
<part-component></part-component>  
</div>  
<script src="../vue.js"></script>  
<script>  
    const partComponent={  
        template:`<button v-on:click="click()">点击次数={{count}}次</button>`,  
        data(){  
            return{  
                count:10  
            }  
        },  
        methods:{  
            click(){  
                this.count++;  
            }  
        }  
    };  
    new Vue({  
        el:'#app',  
        components:{  
            'partComponent':partComponent  
        }  
    });  
</script>  
</body>  
</html>
```
在这个例子中 , 只有id为app的`<div>`标签下的才能使用这个组件
**注意事项**
组件定义需要放置在new Vue()前 , 否则组件注册会失败
## 生命周期和监听函数
**生命周期和钩子函数的概述**
Vue的生命周期指的是一个Vue实例从创建到销毁的过程 , 整个过程分为多个阶段 , Vue为每个阶段提供了对应的钩子函数 , 让我们可以在特定的时间点执行代码. Vue的生命周期大概分为四个主要阶段:
**1.创建 2.初始化数据 3.编译模版 4.挂载DOM 5.渲染-更新-渲染, 6.卸载等一系列过程.**
![](assest/Pasted%20image%2020240918203201.png)
对上面内容的解释[[dom加载和渲染]]
* `new Vue()` new了一个Vue的实例对象 , 此时就会进入组件的创建过程.

* init Events & Lifecycle初始化组件的事件和声明周期

* ==`beforeCreate`== 组件创建之后遇到的第一个声明周期函数 , 这个阶段data和methods以及dom结构都末被初始化, 也就是获取不到data的值 , 不能调用methods中的函数

* `Init injections & reactivity` 这个阶段中 , 正在初始化data和methods中的方法

* ==`created`==这个阶段组件的data和methods中的方法已初始化结束 , 可以访问 , 但是dom结构末初始化, 页面末渲染(这个阶段通常会发起Ajax请求)

* 编译模版结构(在内存)

* ==`beforeMount`== 当模版在内存中编译完成 , 此时内存中的模版结构还末渲染至页面上 , 看不到真实的数据

* `Create vm.$el and replace 'el' with it` 这一步,再在吧内存中渲染好的模版结构替换至真实的dom结构也就是页面上

* ==`mounted`==此时 , 页面渲染好 , 用户看到的是真实的页面数据 , 生命周期创建阶段完毕 , 进入运行中的阶段

2. 生命周期运行中
* ==`beforeUpdate`==当执行此函数 , 数据池的数据新的 , 但是页面是旧的

* `Virtual DOM re-render and patch` 根据最新的data数据 , 重新渲染内存中的模版结构 , 并把渲染好的模版结构 , 替换至页面上

* `updated`页面已经完成了更新 , 此时 , `data`数据和页面的数据都是新的

* beforeDestroy 当执行此函数时,组件即将被摧毁 , 但是还没有真正开始销毁 , 此时组件的`data`,`methods`数据或方法还可被调用

* `Teardown`注销组件和事件监听

* `destroyed`组件已经完成了销毁 
## Vue2脚手架模块开发
### Vue脚手架开发的环境准备
**1.搭建Vue脚手架工程 , 需要使用到NPM(node package manager), npm是随nodejs安装的一款包管理工具 , 类似Mave.**

**npm的简介**
`npm`（Node Package Manager）是 Node.js 的包管理器，它允许你管理 JavaScript 项目中的依赖库。通过 `npm`，你可以：
- **安装包**：下载并安装 JavaScript 代码包，如库或框架（例如 Vue.js、React、Express 等）。
- **管理依赖**：跟踪并管理项目中的外部依赖项。
- **运行脚本**：通过 `npm scripts` 可以自动化运行各种任务，如测试、编译等。
- **发布包**：如果你编写了一个可以复用的库，可以通过 `npm` 发布到 npm 注册表中，供其他开发者使用。

**node.js的简介**
Node.js 是一个基于 **Chrome V8 JavaScript 引擎** 构建的 **JavaScript 运行时环境**。它允许开发者在 **服务器端** 运行 JavaScript 代码，而不仅仅是像传统的 JavaScript 那样只在浏览器端执行。

**webpack的简介**
**Webpack** 是一个**模块打包工具**，广泛用于现代 JavaScript 应用程序的开发。它将项目中的各种资源（如 JavaScript 文件、CSS、图片等）作为模块进行管理，并打包成一个或多个优化后的文件，供浏览器使用。和jar包有一点点的类似

**搭建Vue脚手架流程**
1. 安装node.js 注意 , 安装后要用`node -v`检查一下是否安装成功
2. 安装npm 注意 , 安装前要删除之前的cli版本`npm uninstall vue-cli -g`,然后`npm install cnpm@7.1.0 -g`
3. 然后安装webpack , 先要设置淘宝镜像`npm config set registry https://registry.npm.taobao.org`接着安装webpack
4. 安装Vue-Cli(Vue命令窗口) , 安装指令`npm install -g @vue/cli@4.0.3`
5. 使用webpack创建vue脚手架项目
![](assest/Pasted%20image%2020240920185502.png)
![](assest/Pasted%20image%2020240920185509.png)
最后要运行项目, 指令是`npm run dev`
### Vue项目结构
先把创建好的Vue项目拖到idea打开
![](assest/Pasted%20image%2020240920190132.png)1.config
这个文件里面 , `dev.env.js`是**开发环境的**配置 , `index.js`是主配置文件 , 包括webpack和开发服务器配置, `prod.env.js`**生产**环境的配置 , `test.env.js`测试环境的配置
2.src
`assets/`主要存放静态资源, 如图片 , 字体等 , 会经过Webpack处理
`components/`:存放Vue组件的文件夹, 里面的组件是可以复用的模块化代码
3.router
这个目录下存放路由文件 , 路由器/表
3.App.vue
根组件, 所有的页面和组件都会挂载到这个组件上, 是项目的主体但也, 这里可以显示路由的视图
4.main.js
main.js是核心文件, 应用的入口文件, 通常用来初始化Vue实例, 挂载根组件 , 指定el,router,component,template
5.static/文件夹
用于存放不经过Webpack处理的静态资源 , 例如图标 , 外部库等. 因为这些资源是直接复制到构建输出目录, 不需要打包处理的文件(js的第三方库)
6.package.json
项目的核心配置文件 , 定义了项目的依赖, 版本等信息类似于java的maven
7.index.html
index.html是项目的首页, 这里定义了一个div id=app

**Vue请求界面执行流程**
![](assest/Pasted%20image%2020240922161134.png)
![](assest/Pasted%20image%2020240922161139.png)

**Vue项目结构的总结**
因为VueCLI默认生成的项目代码使用了很多简写, 造成了理解的困难
* 整个页面渲染过程中, main.js是中心 , 也是连接各个组件,路由器的关键
main.js的完整写法
```javascript
import Vue from 'vue'
import App from './App.vue'
import router from './router/index.js'

Vue.config.productionTip=false

new Vue({
	el:'#app',//挂载到index.html的<div id="app"></div>
	router:router,//第二个router是import router from './router'
	components:{'App':App},//因为名字相同,可以省略
	template:'<App/>'//<App/>中的App是上面components引入的组件的名字
})
```

**常用操作, 路由切换**
**1.在component目录下添加vue组件**
```javascript
<script setup>  
  
</script>  
  
<template>  
<div>  
  <h1>Hello ,{{name}}</h1>  
</div>  
</template>  
<script>  
export default{  
  data(){  
    return{  
      name:"mary"  
    };  
  }  
};  
</script>  
<style scoped>  
  
</style>
```
`<template>`标签是用来写html页面的. `<script>`标签是用来写数据池的. `<style scoped>`是用来写css样式的

**2.添加路由 , 在router文件夹下的index.js**
```javascript
import Vue from 'vue'  
import Router from 'vue-router'  
import HelloWorld from '@/components/HelloWorld'  
import hello from "../components/hello.vue";  
  
Vue.use(Router)  
  
export default new Router({  
  routes: [  
    {  
      path: '/',  
      name: 'HelloWorld',  
      component: HelloWorld  
    },  
    {  
      path:'/hello',  
      name:'hello',  
      component:hello  
    }  
  ]  
})
```
注意包的引入
## ElementUI
ElementUI的官网:https://element.eleme.cn/#/zh-CN

1.引入到项目的方法 , 打开项目文件的cmd然后键入`npm i element-ui@2.12.0`,如果是用`idea`更方便 , 在命令窗口键入上面的指令

2.修改main.js
```javascript
// The Vue build version to load with the `import` command  
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.  
import Vue from 'vue'  
import App from './App'  
import router from './router'  
import ElementUI from 'element-ui'  
import 'element-ui/lib/theme-chalk/index.css'  
Vue.use(ElementUI)  
Vue.config.productionTip = false  
  
/* eslint-disable no-new */  
new Vue({  
  el: '#app',  
  router,  
  components: { App },  
  template: '<App/>'  
})
```

3.在自己的vue组件中使用ElementUI组件
```javascript
<script setup>  
  
</script>  
  
<template>  
<div>  
  <h1>Hello ,{{name}}</h1>  
  <el-row>  
    <el-button>默认按钮</el-button>  
    <el-button type="primary">主要按钮</el-button>  
    <el-button type="success">成功按钮</el-button>  
    <el-button type="info">信息按钮</el-button>  
    <el-button type="warning">警告按钮</el-button>  
    <el-button type="danger">危险按钮</el-button>  
  </el-row>  
</div>  
</template>  
<script>  
export default{  
  data(){  
    return{  
      name:"mary"  
    };  
  }  
};  
</script>  
<style scoped>  
  
</style>
```
## Axios
**Axios的概念**
**Axios 是一个基于 Promise** 的 HTTP 客户端，用于浏览器和 Node.js 环境下发送 HTTP 请求。它能够帮助开发者与后端服务器进行交互，获取数据或提交数据。Axios 简单易用，广泛应用于 Vue、React 等前端框架中。
官方文档:https://javasoho.com/axios/
本质是对Promise的封装

example
```javascript
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>myhomework</title>  
</head>  
<body>  
<div id="app">  
    <h1>{{msg}}</h1>  
    <table border="1" width="200">  
        <tr>  
            <td>名字</td>  
            <td>年龄</td>  
        </tr>  
        <tr v-for="monster in monsterList">  
            <td>{{monster.name}}</td>  
            <td>{{monster.age}}</td>  
        </tr>  
    </table>  
</div>  
<script src="../vue.js"></script>  
<script src="../axios.min.js"></script>  
<script>  
    new Vue({  
        el:"#app",  
        data:{  
            msg:"妖怪列表",  
            monsterList:[]  
        },  
        methods:{  
            list(){  
                axios.get("http://localhost:63342/axios/data/response.data.json")  
                    .then(responseData=>{  
                        this.monsterList=responseData.data.data.items;  
                    })  
                    .catch(err=>{  
                        console.log("异常=",err)  
                    })  
            }  
        },  
        created(){  
            this.list();  
        }  
    })  
</script>  
</body>  
</html>
```
**注意点**:要引入vue.js包和axios.min.js这两个包

**格式化输出JSON字符串**
将JSON对象转换成`JSON.stringify(response)`
```javascript
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>myhomework</title>  
</head>  
<body>  
<div id="app">  
    <h1>{{msg}}</h1>  
    <table border="1" width="200">  
        <tr>  
            <td>名字</td>  
            <td>年龄</td>  
        </tr>  
        <tr v-for="monster in monsterList">  
            <td>{{monster.name}}</td>  
            <td>{{monster.age}}</td>  
        </tr>  
    </table>  
</div>  
<script src="../vue.js"></script>  
<script src="../axios.min.js"></script>  
<script>  
    new Vue({  
        el:"#app",  
        data:{  
            msg:"妖怪列表",  
            monsterList:[]  
        },  
        methods:{  
            list(){  
                axios.get("http://localhost:63342/axios/data/response.data.json")  
                    .then(responseData=>{
	                    console.log("responseData=",JSON.stringify(responseData));  
                        this.monsterList=responseData.data.data.items;  
                    })  
                    .catch(err=>{  
                        console.log("异常=",err)  
                    })  
            }  
        },  
        created(){  
            this.list();  
        }  
    })  
</script>  
</body>  
</html>
```
重要是那个`console.log("responseData=",JSON.stringify(responseData))`
这里会经常用到一个网站`https://www.json.cn/` 这个网站可以非常清晰的看懂json字符串形式
![](assest/Pasted%20image%2020240923171319.png)
# Vue3
## Vue3脚手架模块开发
### Vue3脚手架开发的环境准备
**1.安装nvm,以及nvm的介绍**
介绍: 是一个管理Node.js版本的命令行工具,有maven类似的版本管理的功能, 可以让用户在同一台机器上轻松安装和切换不同版本的Node.js.

主要的使用命令
* 切换版本`nvm use <version>` 注意node.js的环境变量
* 安装版本或者卸载版本`nvm install <version>` / `nvm uninstall <version>`
* 查看已安装的版本`nvm ls`
注意 , `<version>`是版本号的占位符

**2.用对应版本的node.js中的npm安装vuecli脚手架**
`npm install -g @vue/cli` 然后打开项目路径cmd指令创建项目`vue create xxx
xxx是项目名的占位符
然后选择创建模式: 选择Manually select features
![](assest/Pasted%20image%2020240924085851.png)
选择需要的插件
![](assest/Pasted%20image%2020240924085913.png)
空格表示选择 , i 表示反向选择 , enter表示下一步
![](assest/Pasted%20image%2020240924085920.png)
选择路由模式
![](assest/Pasted%20image%2020240924090039.png)
选择项目依赖包管理方式
![](assest/Pasted%20image%2020240924090055.png)
    一般是package.json模式 , 这也在vue2的项目结构体系中体现

选择是否保存本次设置\
![](assest/Pasted%20image%2020240924090207.png)

接着在idea中配置npm , 打开项目模版设置
![](assest/Pasted%20image%2020240924090301.png)
### Vue3路由机制
**index.html的解析**
```html
<!DOCTYPE html>  
<html lang="">  
  <head>  
    <meta charset="utf-8">  
    <meta http-equiv="X-UA-Compatible" content="IE=edge">  
    <meta name="viewport" content="width=device-width,initial-scale=1.0">  
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">  
    <title><%= htmlWebpackPlugin.options.title %></title>  
  </head>  
  <body>  
    <noscript>  
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>  
    </noscript>  
    <div id="app"></div>  
    <!-- built files will be auto injected -->  
  </body>  
</html>
```
解析代码模版
`<div id="app"></div>`在body体中只有一个div标签 , 其id为app, 这个id将会连接到`src/main.js`内容 , 然后通过main.js将vue实例化 , 并引入所需要的插件 . 
**main.js**
```javascript
import { createApp } from 'vue'  
import App from './App.vue'  
import router from './router'  
import store from './store'  
  
createApp(App).use(store).use(router).mount('#app')
```
解析这个模版 . 重点是`createApp(App).use(store).use(ruter).mount('#app')`.
* createApp(App)是Vue3中用于创建Vue实例的方法. App是定义的主组件, 通常是`App.vue`是整个vue应用的根组件, 是应用的起点
* `.use(store)`:注册Vuex状态管理, 使其在应用中可用.\
* `.use(router)`:注册Vue Router, 使其路由功能可用.
* `.mount('#app')`:将应用挂载到`#app`Dom元素中 , 渲染`App`组件及其子组件
**App.vue**
```javascript
<template>  
  <nav>  
    <router-link to="/">Home</router-link> |  
    <router-link to="/about">About</router-link>  
  </nav>  
  <router-view/>  
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
```
主组件, 所有的页面都是在App.vue下进行切换的. 这里定义了html的结构, 通常包括子组件的引用. 脚本, 包含组建的逻辑 , 样式. 
作用: 
1.路由视图: 在使用`Vue Router`时, 通常会在`App.vue`中定义`<router-view>`, 用于渲染当前激活的路由组件
2.全局状态管理: 可以在这里引入和使用Vuex来管理全局状态
3.定义了路由规则:定义一个数组 `routes`，其中包含两个路由配置：
- 第一个路由：`/` 代表主页，使用 `HomeView` 组件。
- 第二个路由：`/about` 代表关于页，使用懒加载方式引入 `AboutView` 组件
项目加载的过程 index.html->main.js->app.vue->index.js->hellowrld.vue