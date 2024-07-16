**程序的概念**
程序视为了完成特定任务,用某种语言编写的一组指令的集合,简单说就是我们写的代码[[IDE]]

**进程简介**
1. 进程是指运行中的程序, 比如我们使用QQ,就启动了一个进程, 操作系统就会为该进程分配内存. 当我们使用迅雷, 又启动了一个进程, 操作系统将为迅雷分配新的内存空间
2. 进程是程序的一次执行过程, 或是正在运行的一个程序, 是动态过程: 有它自身的产生, 存在和消亡的过程

**线程的相关概念**
1. 单线程: 同一个时刻, 只允许执行一个线程
2. 多线程: 同一个时刻, 可以执行多个线程,比如: 一个QQ进程, 可以同时打开多个聊天窗口, 一个迅雷进程, 可以同时下载多个文件
3. 并发: 同一时刻,多个任务交替执行, 造成一种"貌似同时"的错觉, 简单说, 单核cpu实现的多任务就是并发
![[Pasted image 20240229133027.png]]
4. 并行: 同一个时刻, 多个任务同时执行. 多核cpu可以实现并行
![[Pasted image 20240229133142.png]]

**创建线程的两种方式**
1.在java中线程来使用有两种方式
1. 继承Thread类, 重写run方法
2. 实现Runnable接口, 重写run方法
*对下列案例说明: 
1. 当一个类继承了Thread类,该类就可以当做线程使用
2. 我们会重写run方法, 写上自己的业务代码
3. run Thread类实现了Runnable接口的run方法
```
@Override
public void run(){
	if(target!=null){
		target.run();
	}
}
```
案例
注意: 是调用start方法,而不是调用run方法, 因为如果是调用run方法,那么并没有增加线程 , 还是调用的是主线程,就会先把run方法执行完毕才向下执行,造成阻塞
```
package com.xinxin.threause;  
  
public class Thread01 {  
    public static void main(String[] args){  
        //创建Cat对象, 可以当做线程使用  
        Cat cat = new Cat();  
        cat.start();//启动线程->最终会执行cat的run方法.  
    }  
}  
  
class Cat extends Thread{  
    int times=0;  
    @Override  
    public void run() {  
        //重写run方法, 写上自己的业务逻辑  
        while(true) {  
            //该线程每隔1秒, 在控制台输出"喵瞄"  
            times++;  
            System.out.println("喵瞄");  
            //让该线程休眠一秒  
            try {  
                Thread.sleep(1000);  
            } catch (InterruptedException e) {  
                throw new RuntimeException(e);  
            }  
            if(times==8)  
                break;  
        }  
    }  
}
```
![[Pasted image 20240229165215.png]]

![[Pasted image 20240229170148.png]]