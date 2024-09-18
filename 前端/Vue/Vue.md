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
## 生命周期和监听函数
**生命周期和钩子函数的概述**
Vue的生命周期指的是一个Vue实例从创建到销毁的过程 , 整个过程分为多个阶段 , Vue为每个阶段提供了对应的钩子函数 , 让我们可以在特定的时间点执行代码. Vue的生命周期大概分为四个主要阶段:
![](assest/Pasted%20image%2020240918203201.png)
对上面内容的解释
* `new Vue()` new了一个Vue的实例对象 , 此时就会进入组件的创建过程.

* init Events & Lifecycle初始化组件的事件和声明周期

* `beforeCreate` 组件创建之后遇到的第一个声明周期函数 , 这个阶段data和methods以及dom结构都末被初始化, 也就是获取不到data的值 , 不能调用methods中的函数

* `Init injections & reactivity` 这个阶段中 , 正在初始化data和methods中的方法

* `created`这个阶段组件的data和methods中的方法已初始化结束 , 可以访问 , 但是dom结构末初始化, 页面末渲染(这个阶段通常会发起Ajax请求)

* 编译模版结构(在内存)

* `beforeMount` 当模版在内存中编译完成 , 此时内存中的模版结构还末渲染至页面上 , 看不到真实的数据

* `Create vm.$el and replace 'el' with it` 这一步,再在吧内存中渲染好的模版结构替换至真实的dom结构也就是页面上

* `mounted`此时 , 页面渲染好 , 用户看到的是真实的页面数据 , 生命周期创建阶段完毕 , 进入运行中的阶段

2. 生命周期运行中
* `beforeUpdate`当执行此函数 , 数据池的数据新的 , 但是页面是旧的

* `Virtual DOM re-render and patch` 根据最新的data数据 , 重新渲染内存中的模版结构 , 并把渲染好的模版结构 , 替换至页面上

* `updated`页面已经完成了更新 , 此时 , `data`数据和页面的数据都是新的

* beforeDestroy 当执行此函数时,组件即将被摧毁 , 但是还没有真正开始销毁 , 此时组件的`data`,`methods`数据或方法还可被调用

