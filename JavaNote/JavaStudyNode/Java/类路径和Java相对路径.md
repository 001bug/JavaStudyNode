### web工程路径的使用背景
我们通常会引用本地文件资源包括:图片,js文件,css文件,web页面... 在引用文件的时候,普遍是2种,相对路径和绝对路径
**相对路径描述**
![](assest/Pasted%20image%2020240726165117.png)
常见的相对路径描述
文档相对路径
```
../xxx 当前文件的上一层目录
./xxx 当前文件所在目录
```
根相对路径
```
/xxx  当前文件所在的根目录
```
**绝对路径的描述**
一些非本地的文件资源，一般都是使用绝对地址的,可以避免环境问题,在实际开发中，路径都使用绝对路径，而不是相对路径, `http://ip:port/工程路径/资源路径`这是绝对路径
**注意**
在web 中`/` 斜杠如果被**浏览器解析**，得到的地址是：`http://ip[域名]:port/`
如果被[服务器](javaweb) 解析，得到的地址是：`http://ip[域名]:port/工程路径/`
### 1.相对路径
相对的是**工作目录** , **工作目录** 本质是**执行命令时，终端（命令行）所在的当前目录**
这里用maven项目举个例子,目录结构如下,**在web工程中,程序实在服务器中执行命令的**

src下是源码，target下是编译后的class文件。我们把hello.txt放在current文件夹下
![](Java/assest/Pasted%20image%2020240716160453.png)
用java输出相对路径
```
public class FilePathDemo {
 
    public static void main(String[] args) {
        File file = new File("hello.txt");
        //判断文件是否存在
        System.out.println(file.exists());
        //获取当前目录
        System.out.println("current dir: " + System.getProperty("user.dir"));
    }
}
```
得到的结果是
![](Java/assest/Pasted%20image%2020240716160616.png)
可以得到的结果
**工作目录**是current文件夹所在路径
![](Java/assest/Pasted%20image%2020240716161647.png)
这里`D:\code\java\current`就是所谓的**工作路径**了,所以说**相对路劲**相对的是工作目录，也就是你执行命令时所在的当前目录。

### 2.类路径
项目中直接用上面的方式读取文件是不合适的,因为用户不同,工作目录也会有所变化 , 所以出现了类路径这个概念
maven管理的项目，默认情况下resource目录下的文件会被放置类路径下。
类路径的绝对路径:D:/code/java/current/target/classes/
        类路径的相对路径其实就是当前class文件所在的路径