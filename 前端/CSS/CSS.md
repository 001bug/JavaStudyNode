## CSS基本的概念
他是一个层叠样式表 , 是一种样式表语言 , 用来描述HTML文档的呈现(美化内容). 
书写位置: 是head标签的子标签, style标签里面书写CSS代码
```html
<title>CSS 初体验</title>
<style>
	/* 选择器 { } */
p {
	/* CSS 属性 */
	color: red;
}
</style>
<p>体验 CSS</p>
```

**CSS引入方式**
* 内部样式表: 学习使用,比如测试某个标签的作用
* 外部样式表: 开发使用,外部样式就是.css文件
* 行内样式: 配合JavaScript???
外部样式表的引入:`<link rel="stylesheet" href="./my.css">` 这个写在head标签下
行内样式适合少量的样式,比如`<button style="color: red;">按钮</button>`
## 选择器
作用:**查找标签**,设置样式并应用样式
#### 基础选择器
**标签选择器**
使用**标签名**作为选择器 → 选中**同名标签**设置**相同的样式**
例如
```html
<style>
 p {
	color: red;
 }
</style>
```
注意:标签选择器会让所有的同一标签变为但一样式
**类选择器**
差异化设置标签的显示效果
步骤:1.定义类选择器->**.类名**
	2.使用类选择器->在html使用类选择器
```css
.red{
	color: red;
}
```
注意事项:
1. 类名自定义，不要用纯数字或中文，尽量用英文命名
2. 一个类选择器可以供多个标签使用
3. 一个标签可以使用多个类名，类名之间用**空格**隔开
```html
<div class="red" "red1">这是div标签</div>
```
**id选择器**
作用：查找标签，差异化设置标签的显示效果。==他一般配合javaScript一起使用,很少单独用于设置CSS样式==
```html
<style>
/* 定义 id 选择器 */
 #red {
   color: red;
 }
</style>
<!-- 使用 id 选择器 -->
<div id="red">这是 div 标签</div>
```
==注意事项:同一个id选择器在一个页面只能使用一次==
**通配符选择器**
作用：查找页面**所有标签，设置相同样式**。很少用
```css
* {
  color: red;
}
```
注意事项:
* 通配符选择器可以用于清除标签的默认样式，例如：标签默认的外边距、内边距

**画盒子**
首次布局,需要画盒子, 用class选择器,主要用三个属性区别和布局,width,height, background-color
#### 组合选择器

^1407d5

定义:由多个基础选择器或两个选择器, 通过不同的方式组合而成.
作用: 更准确, 更高效的选择目标元素
**后代选择器**
作用:选中某元素的后代元素。
选择器写法：父选择器 子选择器 { CSS 属性}.
```html
<style>
 div span {
	color: red;
 }
</style>

<span> span 标签</span>
<div>
	<span>这是 div 的儿子 span</span >
</div>
```
**注意事项 , 后代选择器是先执行子代然后找父代,而且后代都会被选出来**
**子代选择器**
背景
```html
<body>  
<div>  
  <span>div父标签的子标签span</span>  
  <p>  
    <span>P父标签的子标签span</span>  
  </p>  
</div>  
</body>
```
子代选择器: 选中某元素的子代元素(**最近的子级**),不会找其他的后代
选择器写法: 父类选择器>子类选择器`{css属性}`
```html
<style>  
  div>span{  
    color: red;  
  }  
</style>
```
**并集选择器**
并集选择器: 选中**多组**标签设置**相同**的样式。
选择器写法:  选择器1, 选择器2, …, 选择器N { CSS 属性}。
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
    <style>  
      div,p,span{  
        color: aqua;  
      }  
    </style>  
</head>  
<body>  
<div>div标签</div>  
<p>p标签</p>  
<span>span标签</span>  
</body>  
</html>
```
**交集选择器**
交集选择器: 选中同时满足多个条件的元素
环境
```html
<p class="box">p标签, 使用了类选择器</p>
<p>p 标签 </p>
<div class="box">div 标签, 使用了类选择器 box</div>
```
选择器写法：选择器1选择器2 { CSS 属性}，选**择器之间连写，没有任何符号**。
```css
p.box{
  color:red;
}
```
注意:大部分都是**标签.类名**
#### 伪类选择器
伪类选择器:伪类表示**元素状态,** 选中元素的某个状态设置样式.
不是直接选择**元素**(标签)本身，而是选择那些在特定条件下符合某种状态的元素.伪类选择器通常以冒号':'开头
常见伪类选择器 选择器:hover{CSS属性}`[:左右都不能有空格]`
```css
<style>
	a:hover{
		color:red;
	}
	.box:hover{
		color: green;
	}
</style>
<a href="#">a 标签</a>
<div class="box">div 标签</div>
```
上段代码的意思是,鼠标经过a 标签时 , 颜色变为红色
**伪类-超链接(扩展)**
超链接标签一般有四个状态
![](assest/Pasted%20image%2020240729190514.png)
## 文字控制属性

^7bb0f8

文字的控制,主要从文字的形状,大小,颜色,以及位置方面考虑的
#### 总一览图
![](assest/Pasted%20image%2020240728194547.png)
#### 字体类
**字体大小**
属性名: font-size   属性值: 文字尺寸,PC端网页最常用的单位 px
```css
P{
	font-size: 30px;
}
```
经验: 谷歌浏览器默认字号是16px
**字体粗细**
属性值:font-weight 属性值:400不加粗,700加粗
```css
P{
	font-weight: 400;
}
```
**字体样式(是否倾斜)**
作用:清除文字默认的倾斜效果  属性名:font-style  属性值:(不)normal 倾斜(italic)
注意:倾斜的标签是`em`
**行高(行与行之间的距离)**
属性名:line-height 属性值:数字+px （当前标签font-size属性值的倍数）
```css
.font-height{
	line-height: 30px;
}
```
行高计算![](assest/Pasted%20image%2020240728210408.png)
文字垂直**居中**
垂直居中技巧：**行高**属性值**等于**盒子**高度**属性值,
注意：该技巧适用于单行文字垂直居中效果
```css
.box {  
  height: 40px;  
  background-color: #333;  
  color: white;  
  /* 让文字垂直居中的写法  只适合于单行文字*/  
  /* 让行高等于盒子高度即可 */  
  /* line-height: 40px; */  
  /* 1.行高等于盒子高度，则垂直居中 */  
  /* 2.行高小于盒子高度，则文字偏上 */  
  /* 3.行高大于盒子高度，则文字偏下 */  
  line-height: 40px;  
}
```
**字体族**
属性名: font-family 属性值: 字体名 
注意事项:
建议 font-family 属性最后设置一个字体族名
网页开发建议使用无衬线字体
`font-family: Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif;`
以上的一个一个写太过于麻烦,所以可以和着写**font的复合属性**
```css
.font{
	font:倾斜 加粗 字号/行高 字体;
}
```
注意 只有两个地方是必须写的,字号和字体,顺序不能颠倒
**一般的全局配置**
```html
body{
	font:12px/1.5 Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif;
	color:#666;
}
```
#### 文本类
**文本缩进**
属性名:text-indent  属性值:数字+px/数字+em
**文本对齐方式**
作用：控制内容水平(居中)对齐方式
属性名:text-align
属性值:
![](assest/Pasted%20image%2020240729100057.png)
一般的使用是把内容放在div中,然后给div添加属性值
```html
div{
	text-align: center;
}
-----------------------------------
<div>
	<img src="./images/1.jpg" alt="">
</div>
```
**文本修饰线**
属性名: text-decoration
属性值:
![](assest/Pasted%20image%2020240729102206.png)
**文本颜色**
属性名; color
属性值:![](assest/Pasted%20image%2020240729102630.png)
注意:只要属性值为眼液,都可以使用上述的颜色表示方式.
color的设置可以通过rgb模式(xx,xx,xx) color: rgb(244,233,211);红,绿,蓝的比例
background-color:rgba(255,0,0.2);后面的.2表示透明度的意思,会有透明效果
#### 调试工具
快捷键ctrl+f12
上看层叠,下看继承
## CSS特性
**继承性**
继承性: 子级默认继承父级的[文字控制属性](#^7bb0f8)
注意
* 并不是所有的属性都可以继承,只有以文字控制的属性才可继承
* 不只是局限于儿子,所有后代都可以继承
* a标签的**颜色和下划线**不能继承
* h标签的文字大小不能继承
```html
<style>
 .father{
	 width:300px;
	 font-size:20px;
	 text-align:right;
	 background-color:green;
	 color:red;
 }
</style>
<body>
	<div class="father">father标签
		<p>father子标签</p>
	</div>
</body>
```
**层叠性**
特点:相同的属性的前提下, 后面的CSS属性覆盖前面的CSS属性
    不同的属性会叠加, 及所有的CSS属性都会生效
```css
<style>
   div {
	 color: red;
	 font-weight: 700;
   }
   div {
     color: green;
     font-size: 30px;
   }
</style>
<div>div 标签</div>
```
选择器类型相同则遵循层叠性,否则按**选择器优先级**判断
**优先级**
优先级: ==一个标签使用了多个选择器,然后对应的匹配规则==
公式：通配符选择器 < 标签选择器 < 类选择器 < id选择器 < 行内样式 < !important(选中标签的范围越大,优先级越低)
* 优先级-叠加计算规则
```css
div{
	color: red;
}
.box{
	color:green;
}
.box div{
	color:red;
}
.box{
	color: green;
}
```

叠加计算：如果是[复合选择器](#^1407d5)，则需要权重叠加计算。
(**行内**样式, **ID**选择器, **类**选择器个数, **标签**选择器个数)
规则:
* 从左向右依次比较选个数，同一级个数多的优先级高，如果个数相同，则向后比较
* !important 权重最高
* 继承权重最低
![](assest/Pasted%20image%2020240729213915.png)
继承的权重是0 的案例
```html
<style>
	li{
	   color:red;
	}
	.box{
	  color: green;
	}
</style>
<body>
	<ul class="box">
	   <li>我是文字</li>
	</ul>
</body>
```
这里会发现最终的颜色是红色
你会发现,li的权重是1,然后是小于.box的权重是10, 按理说应该是绿色,但是因为li是继承box的,他们是继承关系,所以.box的实际权重是0
## Emment写法
Emmet写法: 代码的简写方式, 输入缩写VS Code会自动生成对应的代码
* HTML![](assest/Pasted%20image%2020240730085035.png)
* CSS: 大多数简写方式为属性单词的首字母
![](assest/Pasted%20image%2020240730085214.png)
背景图	backgroung-image  bgi
## 背景属性
**总属性图**
![](assest/Pasted%20image%2020240730090050.png)
**背景图**
网页中, 使用背景图实现**装饰性**的图片效果.
属性名:background-image(bgi)
属性值:url
```css
div{
  width:400px;
  height:400px;
  background-image: url()
}
```
背景图默认是平铺显示效果

**背景图片的平铺方式**
属性名:background-repeat(bgr)
属性值:
![](assest/Pasted%20image%2020240730091249.png)
水平方向和垂直方向平铺,说的是水平方向不断的添加图片直到占满盒子

**背景图片位置**
属性名: background-position(bgp)
属性值:
* 关键字(常用)
![](assest/Pasted%20image%2020240730091826.png)
```css
div{
  width: 400px;
  height: 400px;
  background-color: pink;
  background-image: url();
  background-repeat: no-repeat;
  background-position: center bottom;
  background-position: 50px -100px;
}
```
第一个是在底部的中间, 第二个是在右上角
* 坐标(数字+px,正负都可以)这跟二维数组的坐标关系差不多
水平: 整数向右, 负数向左
垂直: 整数向下, 负数向上
注意事项:1.关键字可颠倒顺序  2.写一个关键字,另一个方向默认居中; 数字只写一个方向,另一个方向居中
3.一般在设计大的背景图的时候,一般要顶部对齐,然后水平居中,因为这样才能得到更多的信息

**背景图片缩放**
属性名: background-size(bgz)
属性值:
* 关键字
cover: 等比例缩放背景图片以完全覆盖背景区(可能背景图片部分看不见)
contain: 等比例缩放背景图以完全装入背景区, 可能背景区部分空白(图片完整)
* 百分比: 根据盒子尺寸计算图片大小
```css
.box{
  width:400px;
  height:400px;
  background-color: pink;//背景图片是粉色
  background-image:url();
  background-repeat:no-repeat;//平铺效果不重复
  background-size:100% 100%;
}
```
图片能够把背景占满
* 数字+单位(例如:px)
```css
.box{
  width:400px;
  height:400px;
  background-color: pink;//背景图片是粉色
  background-image:url();
  background-repeat:no-repeat;//平铺效果不重复
  background-size:400px 400px;
}
```

**背景图固定**
作用:背景图不会随着元素的内容滚动.
属性名:background-attachment(bga)
属性值:fixed(不滚动) scroll(滚动)
```css
body{
  background-image: url();
  background-repeat: no-repeat;
  background-attachment: fixed;
}
```

**背景复合属性**(类似于字体的复合属性)
属性名: background（bg）
属性值: 背景色 背景图(url) 背景图平铺方式 背景图位置/背景图缩放 背景图固定（空格隔开各个属性值，不区分顺序）
```css
div{
  width:400px;
  height:400px;
  background: pink url() no-repeat right center/cover;
  //想要用背景缩放一定要有图片位置,不然会语法错误
}
```
## 显示模式
概念:标签(元素)的显示方式,决定了元素在页面上的表现形式及布局方式。
#### 常用的显示模式
1. 块级元素
 独占一行
 ==宽度默认是父级的100%==,前提要有继承关系
 添加宽高属性生效
 常见的块级元素:`<div>,<p>,列表表格`
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>块元素</title>
    <style>
      /* 块级元素 */
      /* 1. 独占一行 */
      /* 2. 可以设置宽度和高度 */
      /* 3. 宽度默认和父亲一样宽，如果没有父亲，则和浏览器一样宽 */
      .box1,
      .box2 {
        width: 100px;
        height: 100px;
        background-color: pink;
      }
    </style>
  </head>

  <body>
    <p>abc</p>
    <div class="box1">div1</div>
    <div class="box2">
      <p>123</p>
      <p>123</p>
    </div>
  </body>
</html>
```
2. 行内元素
一行可以显示多个,比如`<a>`是行内元素,然后a可以在一行内存在多个
设置==宽高==属性不生效
宽度尺寸由==内容撑开==,比如把字号设置的大一点

3. 行内块元素
常见的行内块级元素:`<img> <input> <button> <select> <textarea>`
一行可以显示==多个==
设置宽高属性生效
宽高尺寸也可以由内容撑开
#### 转换显示模式
有些时候,需要把行内元素转换为块级元素 , 比如一个链接 , 需要放大点击范围 , 那就需要把行内元素转换为块级元素然后放大
属性名: display
属性值:![](assest/Pasted%20image%2020240730161352.png)
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>Document</title>  
    <style>  
        a{  
            display: inline-block;  
            width: 100px;  
            height: 30px;  
            background-color: pink;  
        }  
        div{  
            display: inline;  
            width: 100px;  
            height: 100px;  
            background-color: red;  
        }  
    </style>  
</head>  
<body>  
<a href="#">"yy"</a>  
<a href="#">"yy"</a>  
<div>123</div>  
<div>123</div>  
</body>  
</html>
```
这里链接本来是没有宽度和高度的,但是经过转换后出现了宽度和高度, 而块级元素div123本来是块级元素一行只能有一个的,但是经过转换,实现了一行多个
