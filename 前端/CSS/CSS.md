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
子代选择器: 选中某元素的子代元素(**最近的子级**)
选择器写法: 父类选择器>子类选择器`{css属性}`
```html
<style>  
  div>span{  
    color: red;  
  }  
</style>
```
#### 伪类选择器
## 文字控制属性
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
垂直居中技巧：行高属性值等于盒子高度属性值,
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
属性名;color
属性值:![](assest/Pasted%20image%2020240729102630.png)
注意:只要属性值为眼液,都可以使用上述的颜色表示方式.
color的设置可以通过rgb模式(xx,xx,xx) color: rgb(244,233,211);红,绿,蓝的比例
background-color:rgba(255,0,0.2);后面的.2表示透明度的意思,会有透明效果
#### 调试工具
快捷键ctrl+f12
