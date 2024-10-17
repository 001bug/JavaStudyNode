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

