1.`<p></p>`
任何段落都要放在`<p></p>`标签中,因为HTML中即使代码换行了,页面显示效果也不会换行,必须写到`<p></p>`中,`<p></p>`标签中不能嵌套h系列的标签和其他p标签
空白折叠现象:
* 文字和文字之间的多个空格,换行会被折叠成一个空格
* 标签"内壁"和文字之间的空格会被忽略
常见的转义字符
![](assest/Pasted%20image%2020240722144801.png)

2.`div`标签
div标签的定义:是英文division分割的缩写 , 用来将相关的内容组合到一起。以便和其他的内容分割。能使文档结构更清晰。
`<div></div>`是最常见的HTML标签,因为它可以结合CSS使用,实现网页的布局,这种布局形式叫`DIV+CSS`,DIV又俗称为盒子
![](assest/Pasted%20image%2020240722155651.png)

3.h系列标签
按级别分为6级`h1,h2,h3,h4,h5,h6`
搜索引擎非常看重`<h1></h1>`标签的内容,应该将重点内容放在`<h1></h1>`中,比如网页的logo等
注意:`<h1></h1>`标签一般只能放置一个,否则会被搜索引擎视为作弊,如果一个页面包含多个 `<h1>` 标签，搜索引擎可能会认为这是试图操纵页面权重或关键词排名，可能视为作弊行为。

4.title标签
titile标签用来设置网页的标题,文字会显示在浏览器的标签栏上
```html
<title>八股文</title>
```

5.meta标签设置网页关键词和描述,name属性用来设置meta的具体功能
![](assest/Pasted%20image%2020240722152446.png)
靠搜索关键词就会出现
模版样例
```html
<!DOCTYPE htm1>
<html lang="en">
	<head>
	    <meta charset="UTF-8">
	    <meta name="viewport" content="width=device-width,initial-scale=1.0">
	    <title></title>
		<meta name="Keywords" content="xxx">
	    <meta name="Description" content="xxxx">
	</head>
	<body>
	</body>
</html>
```
meta标签是元标签用于设置网页的基础设置,head标签是网页的配置,body标签是网页的内容

配置vs
![](assest/Pasted%20image%2020240722154610.png)
HTML骨架的生成:输入!,按tab键,即可自动生成HTML5的骨架