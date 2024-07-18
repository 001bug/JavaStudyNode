坐标体系-介绍
下图说明了java坐标系, [坐标原点位于左上角], 以像素为单位.在java坐标系中,第一个是x坐标,表示当前位置为水平方向,距离坐标原点x个像素; 第二个是y坐标, 表示当前位置为垂直方向, 距离坐标原点y个像素
![[Pasted image 20240227135339.png]]
**像素的概念**
1. 计算机在屏幕上显示的内容都是由屏幕上的每个像素组成的, 例如,计算机显示器的分辨率是800 * 600,表示计算机屏幕上的每一行由800个点组成,共有600行,像素是[密度单位].

**绘图步骤**
总体来说就是 1.画好图形   2.建好画板然后把图形接上
```
package com.xinxin.draw;  
  
import javax.swing.*;  
import java.awt.*;  
  
public class drawmap extends JFrame{//JFrame对应窗口,可以理解成是一个画框  
  
    //定义一个面板  
    private MyPanel mp=null;  
    public static void main(String[] args){  
        new drawmap();  
    }  
    public drawmap(){//构造器具有初始化的作用  
        //初始化面板  
        mp=new MyPanel();  
        //把面板放入到窗口(画板)  
        this.add(mp);  
        //设置窗口的大小  
        this.setSize(800,800);  
        this.setVisible(true);//表示可以显示  
    }  
}  
  
class MyPanel extends JPanel{  
    @Override  
    public void paint(Graphics g) {  
        super.paint(g);  
        g.drawOval(10,10,100,100);//调用g的方法  
    }  
}
```

**绘图原理**
1.Component类提供了两个和绘图相关最重要的方法
1. paint(Graphics g)绘制组件的外观
2. repaint()刷新组件的外观
2.当组件第一次在屏幕显示的时候,程序会自动的调用paint()方法来绘制组件    当以下情况paint()将会被调用:
1. 窗口最小化,再最大化
2. 窗口大小发生变化
3. repaint方法被调用