### 1.相对路径
相对的是**工作目录** , **工作目录** 本质是**执行命令时，终端（命令行）所在的当前目录**
这里用maven项目举个例子,目录结构如下

src下是源码，target下是编译后的class文件。我们把hello.txt放在current文件夹下
![](assest/Pasted%20image%2020240716160453.png)
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
![](assest/Pasted%20image%2020240716160616.png)
可以得到的结果
**工作目录**是current文件夹所在路径
![](assest/Pasted%20image%2020240716161647.png)
这里D:\code\java\current就是所谓的**工作路径**了,所以说**相对路劲**相对的是工作目录，也就是你执行命令时所在的当前目录。

### 2.类路径
项目中直接用上面的方式读取文件是不合适的,因为用户不同,工作目录也会有所变化 , 所以出现了类路径这个概念
maven管理的项目，默认情况下resource目录下的文件会被放置类路径下。
类路径的绝对路径:D:/code/java/current/target/classes/
        类路径的相对路径其实就是当前class文件所在的路径