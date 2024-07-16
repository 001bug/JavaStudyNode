实现Runnable接口
1. java是单继承的, 在某些情况下一个类可能已经继承了某个父类,这时再用继承Thread类方法来创建线程显然不可能了.
2. java设计者们提供了另外一个方式创建线程, 就是通过实现Runnable接口来创建线程.
T2 t2 =new T2();
这句话是重点, 这里的底层使用了设计模式[代理模式]
```
package com.xinxin.threause;  
  
//通过实现接口Runnable来开发线程  
public class Thread02 {  
    public static void main(String[] args){  
        Dog dog = new Dog();  
        Thread thread = new Thread(dog);//传一个类  
        thread.start();  
    }  
}  
  
class Dog implements  Runnable{//通过实现Runnable接口,开发线程  
    int count =0;  
    @Override  
    public void run() {  
        while(true){  
            System.out.println("小狗旺旺叫"+(++count)+Thread.currentThread().getName());  
            //休眠1秒  
            try {  
                Thread.sleep(1000);  
            } catch (InterruptedException e) {  
                throw new RuntimeException(e);  
            }  
  
            if(count==10){  
                break;  
            }  
        }  
    }  
}
```

模仿代理模式
```
class ThreadProxy implements Runnable{//线程代理类  
    private Runnable target =null;  
  
    @Override  
    public void run() {  
        if(target!=null){  
            target.run();  
        }  
    }  
    public ThreadProxy(Runnable target){  
        this.target=target;  
    }  
    public void start(){  
        start0();//这个方法才是真正实现多线程的方法  
    }  
    public void start0(){  
        run();  
    }  
}
```

**继承Thread实现线程和实现Runnable线程的区别**
1. 从java的设计来看, 通过继承Thread或者实现Runnable接口来创建线程本质没有区别,从jdk帮助文档可知Thread类本身就实现了Runnable接口的用start()方法去调用start0()
2. 实现Runnable接口方式更加适合多个线程共享一个资源的情况, 并且避免了单继承的限制,建议使用Runnable接口去实现多线程