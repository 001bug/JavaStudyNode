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

