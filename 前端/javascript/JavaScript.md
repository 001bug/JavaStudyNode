# JavaScript基本介绍
## js概念
官方文档: https://www.w3school.com.cn/js/index.asp

基本说明: js是一种**高级 , 动态 , 弱类型的脚本语言**. 脚本语言指的是不需要经过编译 , 可以直接运行的编程语言. C,C++这种编译语言是需要通过编译器转成二进制文件然后运行的 . 而脚本语言通过解释器直接运行源码. JS能改变HTML内容 . 改变HTML属性 , 改变HTMl样式

**改变HTML属性 , HTML内容 , CSS样式**
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>js dom操作实例</title>  
    <style>  
        #myDiv{  
            width: 100px;  
            height: 100px;  
            background-color: lightblue;  
        }  
    </style>  
</head>  
<body>  
    <h1 id="heading">原始标题</h1>  
    <p id="text">原始段落内容</p>  
    <div id="myDiv">原始内容</div>  
    <button onclick="changeContent()">改变内容</button>  
    <button onclick="changeAttribute()">改变属性</button>  
    <button onclick="changeStyle()">改变样式</button>  
  
    <script>  
        function changeContent(){  
            document.getElementById("heading").innerHTML="改变内容";  
        }  
        function changeAttribute(){  
            document.getElementById("myDiv").setAttribute("title","这是一个新提示消息");  
            alert("属性已改变 , 现在鼠标悬停在方块上会显示提示信息");  
        }  
        //改变样式  
        function changeStyle(){  
            document.getElementById("myDiv").style.width="200px";  
        }  
    </script>  
</body>  
</html>
```
**弱类型语言**
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
</head>  
<body>  
<script>  
    num=1;  
    console.log("num_type="+typeof(num));//num  
    num="string";  
    console.log("num_type="+typeof(num));//string
    num=1;  
    console.log("num_type="+typeof(num+"string"));//string  
</script>  
</body>  
</html>
```
## js快速入门
**使用script标签写js代码**
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
</head>  
<body>  
<script>  
    num=1;  
    console.log("num_type="+typeof(num));//num  
    num="string";  
    console.log("num_type="+typeof(num));//string
    num=1;  
    console.log("num_type="+typeof(num+"string"));//string  
</script>  
</body>  
</html>
```
细节介绍: 
可以在head和body嵌入script , 执行顺序是从上到下的, 建议放在head中

**使用script标签引入JS文件**
no2.html
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
</head>  
<body>  
<script type="text/javascript" src="../script/text.js"></script>  
</body>  
</html>
```
text.js
```javascript
num=1;  
alert("num_type="+typeof(num));  
num="string";  
console.log("num_type="+typeof(num));  
num=1;  
console.log("num_type="+typeof(num+"string"));
```
注意细节; 目录 `./`表示当前目录 , `../`表示当前目录的上一级
以上的两种方法是不能够混用的
# JavaScript变量
## 变量的基本介绍
JavaScript变量表示存储数据的容器
![](assest/Pasted%20image%2020241017210225.png)
**与java变量的对比**
* 声明类型
js使用`var`,`let`和`const`关键字来定义变量 , 变量在定义后赋值 , 且不需要声明类型,动态语言 , 类型运行时可以随时改变; 
java变量必须声明其类型 , 并使用相应的类型关键字.
* 作用域
js中`var`定义的变量有函数作用域 , `let`和`const`定义的变量具有块作用域.
```javascript
if (true) {
    var x = 5; // 函数作用域
    let y = 10; // 块作用域
}
console.log(x); // 输出 5
console.log(y); // 报错：y is not defined
```
java变量的作用域通常基于类和方法的.
## JS的数据类型
**数据类型介绍**
数值类型: number
字符串: string
对象类型: object
布尔类型: boolean
函数类型: function
undefined: 变量末赋值初始值时 , 默认undefined
null: 空值
NaN: Not a Number 非数值

**数据类型注意事项**
String字符串的界定符可以是`''`也可以是`""` 

**运算符**
javascript的算术运算符和赋值运算符和java都是一样的. 关系运算符有一点区别
![](assest/Pasted%20image%2020241018081204.png)
这个`===`全等运算符 . `==`是简单的做字面值比较 , 而全等于 , 除了做字面值比较之外 , 还会比较两个变量的数据类型

细节:
在javascript中 , 所有的变量 , 都可以作为一个boolean类型的变量去使用 , `0`,`undefined` , `""`(空串) 都认为是false

**数组的定义**
在javascript中 , 数组是一种特殊的对象 , 用于存储一组有序的数据 , 可以使用不同的方法来定义数组
1.使用方括号`[]`来定义数组(常见):
```javascript
//方式1
let fruits = ['apple','banana','orange'];
console.log(fruits[1])//下标访问

//方式2
var cars = [];
cars[0] = '奥迪';
cars[1] = '宝马';
```
2.使用`Array`构造函数
```javascript
//方式1
var cars = new Array("aa","bb",'cc');
console.log(cars);

//方式2
var cars1 = new Array();
cars1[0] = "kjk";
cars1[1] = "jj";
```
和java明显的不同就是不需要定义数组长度, 而且没有指定类型

**数组使用和遍历**
遍历
```javascript
var cars = ["aa","bb",100];
console.log(cars.length)
for(i = 0;i<cars.length;i++){
	console.log(cars[i]);
}
```
## Javascript函数
**Javascript函数快速入门**
1.JS函数的定义
函数是一段可以重复执行的代码 , 是由**事件驱动**的(Java的函数是靠类的实例或静态类的本身,控制流等驱动的)
2.快速入门
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
    <script>  
        function hi() {  
            alert("hi");  
        }
        hi(); //主动调用  
    </script>  
</head>  
<body>  
<button onclick = "hi()">点击这里</button>  
</body>  
</html>
```
细节:
* 如果不调用hi()函数 , 是不会触发的. js中如果想要执行函数 , 有两种方式1.主动调用2.通过事件去触发
* 这里给button绑定了onclick事件 , 当用户点击button , 就会事件触发hi()函数

**函数的定义方式以及细节**
1.函数声明
这是常见的函数定义方式 , 通过`function`关键字定义一个函数
```javascript
function greet(name){
	return "hello "+name;
}
console.log(greet("jhon"));
```
![](assest/Pasted%20image%2020241018085529.png)
与java巨大的区别 , 没有指定返回参数 , 形参不需要指定类型 , 返回类型取决于返回的是什么

2.函数表达式(将函数赋给变量)
将函数赋值给一个变量，称为函数表达式
```javascript

```