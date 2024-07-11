**标题**
1. 标题使用< h1 >-< h6 >标签进行定义,< h1 > 定义最大的标题,< h6 >定义最小的标题

**超链接标签**(超有用)
超链接标签
1. 超链接是指从一个网页指向一个目标的链接关系, 这个目标可以是另一个网页, 也可以是相同网页上的不同位置, 可以是一个图片,一个电子邮件地址, 一个文件, 甚至是一个应用程序
2. 应用实例
```
<!--文档类型说明-->  
<!DOCTYPE html>  
  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>超链接标签</title>  
</head>  
<body>  
   <a href="http://www.sohu.com",target="_blank">搜狐</a>  
<!--解读:  
a 标签 是超链接  
href 属性设置连接的地址  
target 属性设置哪个目标进行跳转  
    _self : 表示当前页面(默认值), 即使用当前替换目标页  
    _blank : 表示打开新页面来进行跳转-->  
</body>  
</html>
```

**无序列表**
1. 基本语法
< ul type="属性" >
< li >列表内容< /li >
< /ul >
解读, < UL >设定符号款式, 其值有三种,如下:
		默认为type="disc":
		type="circle"时的列项符号为空心圆
		type="square"时的列项符号为空心正方形
2. 实例
```
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>ul li标签</title>  
</head>  
<body>  
<h1>小学生</h1>  
<ul type="disc">  
    <li>小明</li>  
    <li>小华</li>  
    <li>小花</li>  
</ul>  
</body>  
</html>
```

**有序列表**
1. 语法
	ol: 表示有序列表
	li: 列表项
	type属性: 指定列表项前排序方式
	type 设定数目款式, 其值有五种, 默认为start="1"
	i可以取以下值中的任意一个
	1 阿拉伯数字 1,2,3,....
	a 小写字母a, b, c,.....
	A 大写字母A,B,C,.....
	i小写罗马数字i,ii,iii...
	I大写罗马数字I,III,III,...
2. 实例\
```
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>ul li标签</title>  
</head>  
<body>  
<h1>小学生</h1>  
<ol type="I">  
    <li>小明</li>  
    <li>小华</li>  
    <li>小花</li>  
</ol>  
</body>  
</html>
```

**图像标签**
1. img 标签可以在html页面上显示图片
2. 实例
```
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
</head>  
<body>  
<h1>图片标签显示</h1>  
<!--./imgs/text.png表示当前路径下的imgs文件夹下的text.png-->  
<img src = "./imgs/text.png" height="100">  
</body>  
</html>
```
注意事项:
1. wight: 属性设置图片的宽度
2. height: 属性设置图片的高度
3. border: 属性设置图片边框大小
4. alt: 属性设置当指定路径找不到图片时,用来指代显示的文本内容
相对路劲: 从工程名开始算
绝对路劲: 盘符: /目录/文件名

在web中路劲分为相对路劲和绝对路劲两种
相对路径: . 表示当前文件所在的目录
		 . . 表示当前文件所在的上级目录
文件名 : 表示当前文件所在的目录的文件, 相当于 ./文件名 ./ 可以省略
绝对路径: 正确格式是: http: //IP地址: port/工程名/资源路径
