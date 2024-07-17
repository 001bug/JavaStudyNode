数据类型介绍
数值类型：     number
字符串类型： string
对象类型:        object
布尔类型：     boolean
**函数类型：     function**

特殊值!!!(最能体现出js弱语言的特性)
undefined                 变量未赋初始值时，默认undefined
null                           空值
NaN    或    Not a Number 非数值

for example
```
</head>
<body>
<script type="text/javascript">
//老韩说明
//1. typeof()是JavaScript 语言提供的一个函数。
//2. 返回变量的数据类型
//3. 3 个特殊值undefined 没有赋值就使用null, NaN 当不能识别类型
var email; //特殊值undefined
console.log("email= " + email);//undefined
var address = null;
console.log("address= " + address);//null
console.log(10 * "abc");//NaN= Not a Number
</script>
</body>
</html>
```


