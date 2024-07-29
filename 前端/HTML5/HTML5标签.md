## 简单的内容标签
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

6.换行与水平线标签
![](assest/Pasted%20image%2020240726162501.png)
水平线：`<hr>`（单标签）

7.文本格式化标签
作用:为文本添加特殊格式，以突出重点。常见的文本格式：加粗、倾斜、下划线、删除线等。
![](assest/Pasted%20image%2020240726162850.png)
```html
<strong>strong</strong>
<em>strong</em>
```

8.图像标签
快速使用
作用：在网页中插入图片。
```html
<img src="图片的 URL">
```
以 ./ 开头，VS Code 有提示功能
图像标签属性
![](assest/Pasted%20image%2020240726163229.png)
![](assest/Pasted%20image%2020240726163336.png)
9.超链接
点击跳转到其他页面。
```html
<a href="https://www.baidu.com">跳转到百度</a>
```
**超链接默认是在当前窗口跳转页面**，添加 `target="_blank"` 实现**新窗口**打开页面。
拓展：开发初期，不确定跳转地址，则 href 属性值写为` #，表示空链接，页面不会跳转`

10.音频标签
```html
<audio src="音频的 URL"></audio>
```
常见属性
![](assest/Pasted%20image%2020240726170549.png)

11.视频标签
```html
<video src="视频的 URL"></video>
```
常见属性
![](assest/Pasted%20image%2020240726170859.png)
## 列表,表格,表单
#### 列表
列表的定义
作用：**布局内容排列**整齐的区域。
列表分类：无序列表、有序列表、定义列表。
![](assest/Pasted%20image%2020240727154307.png)
其中最为重要的是无序列表

**无序列表**
作用：布局排列整齐的不需要规定顺序的区域。
标签：ul 嵌套 li，ul 是无序列表，li 是列表条目
```html
<ul>
	<li>第一项</li>
	<li>第二项</li>
	<li>第三项</li>
……
</ul>
```
li 标签可以包裹任何内容 , ur标签只能放`<li>`标签 

**有序列表**
==不常用==
作用：布局排列整齐的需要规定顺序的区域。
标签：ol 嵌套 li，ol 是有序列表，li 是列表条目。
```html
<ol>
	<li>第一项</li>
	<li>第二项</li>
	<li>第三项</li>
……
</ol>
```
li 标签可以包裹任何内容 , ol标签只能放`<li>`标签

**自定义列表**
标签：dl 嵌套 dt 和 dd
**dl** 是定义列表，**dt** 是定义列表的**标题**，**dd** 是定义列表的**描述**
```html
<dl>
	<dt>列表标题</dt>
	<dd>列表描述 / 详情</dd>
	……
</dl>
```
dl 里面只能包含dt 和 dd,子标签可以包含任何内容,就像树形结构,要特别注意他们的关系
![](assest/Pasted%20image%2020240727160807.png)
服务中心是`<dt>`标签 , 往下就是`<dd>`标签的自定义的内容

#### 表格
网页中的表格与 Excel 表格类似，用来**展示数据**。
标签：==table 嵌套 tr==，tr 嵌套 td / th.
![](assest/Pasted%20image%2020240727191658.png)
table是父节点,tr是table的子节点,th和td是同级节点
注意:在默认情况下 ，表格默认没有边框线，使用 border 属性可以为表格添加边框线

**表格结构标签类似于border属性**
![](assest/Pasted%20image%2020240727192634.png)
上表的标签都是table的子标签,tr的父标签
```html
<body>
	<table>
		<thead>
			<tr>
				<th>标签名</th>
				<th>含义</th>
				<th>特殊说明</th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<th>thead</th>
				<th>表格头部</th>
				<th>表格头部内容</th>
			</tr>
		</tbody>
		<tfoot>
			<tr>
				<th>tbody</th>
				<th>表格主体</th>
				<th>主要内容区域</th>
			</tr>
		</tfoot>
	</table>
</body>
```

**合并单元格**
作用：将**多**个**单元格**合并成**一个单元格**，以**合并同类信息**.
1.写出原生表,并明确合并目标
![](assest/Pasted%20image%2020240727204655.png)
2.跨行合并
![](assest/Pasted%20image%2020240727204737.png)
```html
<body>  
    <table border="2">  
        <tr>  
            <td rowspan="2">标签名</td>  
            <td>含义</td>  
            <td>特殊说明</td>  
        </tr>  
        <tr>  
            <td>表格头部</td>  
            <td>表格头部内容</td>  
        </tr>  
        <tr>  
            <td>tbody</td>  
            <td>表格主体</td>  
            <td>主要内容区域</td>  
        </tr>  
</body>
```
**rowspan**表示被覆盖的**单元格**有多少个(包括本身)
3.跨列合并
![](assest/Pasted%20image%2020240727204816.png)
总体原则\
1. 明确合并的目标
2. 保留最左最上的单元格，添加属性（取值是数字，表示需要合并的单元格数量）
– 跨行合并，保留最上单元格，添加属性 rowspan
– 跨列合并，保留最左单元格，添加属性 colspan
3. 删除其他单元格
注意：不能跨表格结构标签合并单元格（thead、tbody、tfoot）
#### 表单
表单用于收集用户信息
使用场景：• 登录页面• 注册页面• 搜索区域

**input标签**
作用以及特点:input 标签 type 属性值不同，则功能不同
`<input type="..." >`
![](assest/Pasted%20image%2020240727212518.png)
* input标签(占位文本)->(提示信息)
![](assest/Pasted%20image%2020240727212728.png)
`<input type="..." placeholder="提示信息">` 文本框和密码框都可以使用
* 单选框 radio
常用属性:![](assest/Pasted%20image%2020240727212942.png)
```html
<input type="radio" name="gender" checked> 男
<input type="radio" name="gender"> 女
```
name的属性值是自己定的,可以用在css的协作上,有相同name就说明单选框是同一组,只能选一个
* 多选框-checkbox
默认选中：checked。
```html
<input type="checkbox" checked> 敲前端代码
<input type="checkbox"> 敲后端代码
```
* 文件上传-file
默认只能上传**一个**文件, 添加**multiple属性**可以实现文件多选功能.
`上传文件:<input type="file" multiple>`

**下拉菜单**
![](assest/Pasted%20image%2020240727220344.png)
标签：**select 嵌套 option**，select 是下拉菜单整体，option是下拉菜单的每一项。
select是跟标签,option都是同级子标签, option有个selected的属性,这是默认选中的标志,比如最恶心的填写信息中,选省份不能通过定位,然后默认选定一个

**文本域**
作用：多行输入文本的表单控件。
标签：textarea，双标签。
```html
<textarea>默认提示文字</textarea>
```
在实际应用中,使用css设置文本域的尺寸大小,还要禁用右下角的拖拽功能
![](assest/Pasted%20image%2020240727221707.png)

**label标签**
网页中，某个标签的说明文本。![](assest/Pasted%20image%2020240728140401.png)
比如这个中国大陆+86就是label标签造成的效果
经验：用 label 标签绑定文字和表单控件的关系，==增大表单控件的点击范围==。
label标签-增大点击范围
* label标签只包裹内容,不包裹表单控件 , 设置**label标签的**for属性值和表单控件的id属性值相同(label标签和表单标签是同级关系)
```html
<input type="radio" id="man">
<label for="man">男</label>
```
这里造成的效果是:我点击男这个文本 , 会有选择男的效果,注意两边都要有对应的id这个属性去链接属性
* 使用label标签包裹文字和表单控件.
```html
<label><input type="radio"> 女</label>
```
label标签是父节点,表单标签是子节点
支持 label 标签增大点击范围的**表单控件**：文本框、密码框、上传文件、单选框、多选框、下拉菜单、文本域等等。

**button**
`<button type="">按钮</button>`
type属性值的描述
![](assest/Pasted%20image%2020240728145320.png)
对于type属性,要配合**form标签**才能实现对应的功能
## 语义化
语义化:是指使用具有明确含义的标签来标记文档结构和内容,帮助人理解代码,帮助搜索引擎更好的解析和理解页面的内容**div就是个盒子,是用来布局的**
#### 无语义的布局标签
作用：**布局网页**（划分网页区域，摆放内容）
div: 独占一行  span: 不换行(span可以一行多个)
#### 有语义的布局标签
![](assest/Pasted%20image%2020240728152521.png)
一般都是按这种模块进行开发的
## 字符实体
作用:在网页中显示预留字符
![](assest/Pasted%20image%2020240728153049.png)
It是less than的缩写
gt 是greater than的缩写