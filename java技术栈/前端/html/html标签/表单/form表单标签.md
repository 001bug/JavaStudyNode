基本语法
```
< from action ="url" menthod=*>
...
...
< input type = submit > <input type=reset>
</form>
```
url表示定位一个web资源的路径,星号部分可以为GET,也可以是POST,method主要有两种get,post

实例
```
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
</head>  
<body>  
<!--  
    1.form 表示表单  
    2.action: 提交到哪个页面  
    3.method: 提交方式, 常用get和post  
    4.input type = text 输入框  
    5.input type = password 密码框  
    6.input type = submit 提交按钮  
    7.input type = reset 重置按钮  
  
    为了个汉字对齐,输入全角空格  
-->  
<h1>登录系统</h1>  
<form action = "OK.html" method="get">  
用户名:<input type = "text" name = "username"><br/>  
密  码:<input type ="password" name = "username"><br/>  
    <input type="submit" value="登录"> <input type ="reset" value="重新填写">  
</form>  
</body>  
  
</html>
-------------------------------------------------------------------------```
```OK.html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
</head>  
<body>  
    <h1>恭喜你,登录OK</h1>  
</body>  
</html>
```

