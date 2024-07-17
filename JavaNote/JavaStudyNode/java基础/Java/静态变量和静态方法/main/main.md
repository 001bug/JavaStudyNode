**main方法的语法**
解释main静态方法的形式 : public static void main(String[] args){}
1.java虚拟机需要调用类的main()方法,所以该方法的访问权限必须是public 
[因为虚拟机在调用的时候根本和main方法不是同一个类,所以要用public]
2.java虚拟机在执行main()方法时不必创建对象,所以该方法必须是static
3.该方法接受String类型的数组参数, 该数组中保存执行java命令时传递给所运行的类的参数
(java 执行程序 参数1 参数2 参数3)
![[Pasted image 20240212173828.png]]
4.main方法是虚拟机调用

**main方法的注意事项**
1.在main()方法中,我们可以直接调用main方法所在类的静态方法或静态属性.
2.但是,不能直接访问该类中的非静态成员, 必须创建该类的一个实例对象后,才能通过这个对象去访问类中的非静态成员

**main动态传值**
在[[IDE]]中传递main方法参数
右上角点击方法名--->Edit Configurations--->点击Program arguments