## 常用集合
#### ArrayList
**sort**
1.默认排序:
* 全局排序(默认是升序的23)
```java
Integer[] arr = {5,4,7,9,2,12,54,21,1};
Arrays.sort(arr);
```
* 局部排序
```java
public static void sort(T[] a, int fromIndex, int toIndex)
```
2.自定义排序,包括对对象的排序和排序形式
想要实现自定排序,需要传入一个实现了Comparator接口的实现类
* 匿名内部类的形式
```java
Integer[] arr = {1,5,8,4,2,6,4};
Arrays.sort(arr, new Comparator<Integer>()
{
	@Override
	public int compare(Integer o1, Integer o2) {
		return o2-o1;
	}
});
```
分析:如果o2大于o1(o1<o2)升序,那么o2-o1为正数,返回真,交换位置o2>o1
	相反,如果o2小于o1(o1>o2)降序,那么o2-o1为负数,返回假,不交换位置
* 实现接口类的形式
```java
class MyComparator implements Comparator<Integer>{  
    @Override  
    public int compare(Integer o1, Integer o2) {  
       return o2-o1;  
    }  
}
```
## 反射机制
#### 反射机制的概述
示意图
![](assest/Pasted%20image%2020240717100303.png)
对示意图的解释
反射机制允许程序在执行时,通过API获得类的内部信息,并且能操作对象的属性以及方法,可以动态的创建对象, **也有缺点**:是解释执行, 对执行速度有影响
本质上,在类加载器加载完类之后,在堆会产生一个类对象(这个类对象是每个类独属) , 类对象包含了类的完整的结构信息 , 就像镜子一样,所以俗称为反射
#### 反射的主要功能以及类
1.主要功能
![](assest/Pasted%20image%2020240717100525.png)
2.主要类
![](assest/Pasted%20image%2020240717100553.png)
Class类的对象是系统创造的.
#### 反射各类的大概介绍
* **Class方面**
1.获取Class类对象
```
//1. Class.forName  
String classAllPath = "全类名";   
Class<?> clazz = Class.forName(classAllPath);

//2. 类名.class , 应用场景: 用于参数传递  
Class clazz = 类名.class;

//3. 对象.getClass(), 应用场景，有对象实例
Class clazz = 类的实例.getClass();

//4. 通过类加载器【4 种】来获取到类的Class 对象  
ClassLoader classLoader = 类名.getClass().getClassLoader();  
Class clazz = classLoader.loadClass(全类名);

//5.通过Method获得
Class clazz = method.getDeclaringClass();
```
2.通过Class对象得到类的包名,全类名,运行类型,创建对象, 获取属性, 属性赋值...
```
String classAllPath = "全类名";  
Class<?> cls = Class.forName(classAllPath);
cls.getPackage().getName()//包名
cls.getName()//全类名
cls.getClass()//返回运行类型
类 xxx = (类) cls.newInstance();//创建对象
Field brand = cls.getField("属性名");//获取属性
字段(Field).set(对象实例, 值);//要先获取字段(反射获取),有对象实例
```
![](assest/Pasted%20image%2020240717102816.png)

* **reflect.Method方面**
![](assest/Pasted%20image%2020240717103023.png)
5.getDeclaringClass()返回的是class对象

* **reflect.Field方面**  
![](assest/Pasted%20image%2020240717102859.png)

#### 通过反射创建对象
```
Class<?> clazz = Class.forName("全类名");//先获取Class类  
Object o = clazz.newInstance();
```
