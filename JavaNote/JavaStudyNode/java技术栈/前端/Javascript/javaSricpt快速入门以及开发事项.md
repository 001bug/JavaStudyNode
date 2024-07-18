1.官方文档: https://www.w3school.com.cn/js/index.asp:
2.离线文档: W3School 离线手册(2017.03.11 版).chm

JavaScript在开发中的主要作用, JavaScript能改变HTML内容, 能改变HTML属性, 改变HTML样式(CSS), 能完成行为, 页面的数据验证

**JavaScript特点**
1.JavaScript是一种解释型的脚本语言, c,c++等语言都是首先编译后执行, 而JavaScript是在程序的运行过程中逐行进行解释
2.JavaScript是一种基于对象的脚本语言,可以创建对象，也能使用现有的对象(有对象)。
3.javaScript 是弱类型的，对变量的数据类型不做严格的要求，变量的数据类型在运行过程
可以变化。
 ```
 <!DOCTYPE html>  
<html lang="en" xmlns="http://www.w3.org/1999/html">  
<head>  
    <meta charset="UTF-8">  
    <title>弱类型的特性</title>  
    <script type="text/javascript">//表示此脚本语言是javascript的  
        var name="zhou";  
        name=100;  
        alert(name);//先把字符类型赋值给name, 再把数值类型赋值给name,在java这种强制性语言是不同的,他是弱类型语言  
    </script>  
</head>  
<body>  
    <h2>JavaScript的作用</h2>  
    <d></d>  
    <p>能改变HTML属性值</p>  
  
</body>  
</html>
```

**快速入门,编写js代码**
1.使用方式 1:script标签 写js代码//这个标签可以放在body中或者head中都可以

2.使用方式 2:使用script标签[引入JS文件](html常用标签)

以上两种方法只能用一次,如果写了两种 , 不会报错(体现出弱类型语言的特性) , 但是只会生效一次 , 前面引入的先生效

**用浏览器查看报错信息的方式 , 更多工具--> 开发者-->console 或者快捷键 ctrl+shift+i**

[[js基础]]



