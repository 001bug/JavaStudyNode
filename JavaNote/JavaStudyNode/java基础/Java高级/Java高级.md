# 变量
## 变量的介绍
**变量的定义** 变量是程序的基本组成单位. 几乎所有的高级程序语言都是由(类型+名称+值)变量相当于内存中一个数据存储空间

**变量的示意图**
![](assest/Pasted%20image%2020241104210948.png)

**程序中的+号**
![](assest/Pasted%20image%2020241104211157.png)
## 数据类型
**数据类型体系图**
![](assest/Pasted%20image%2020241104211554.png)

**整数类型**
![](assest/Pasted%20image%2020241104211648.png)
细节
1.java整数类型有固定的范围和字段长度, 不受OS(操作系统)的影响, 这是为了保证java程序的可移植性
2.java的整型常量(数字) 默认是int, 如果要声明long型常量须加`l`或`L`

**浮点类型**
![](assest/Pasted%20image%2020241104212030.png)
细节
1.浮点数在机器中存放形式: 浮点数=符号位+指数位+尾数位
2.尾数位部分可能会丢失, 造成精度损失
3.java的浮点型常量默认是double型, 声明float型常量, 须在其后加`f`或`F`
4.浮点数的表达形式
* 十进制数形式: 如 5.12  512.0f
* 科学技术法形式如: `5.12e2(5.12*10^2)` `5.12E-2(5.12/10^2)`
5.浮点数使用陷阱: 2.7和8.1/3比较(2.7大)
因为浮点数是以二进制形式存储, 十进制的小数无法精确地转换成二进制. 这会引起一定的精度误差.因此要有一个容错范围
```
double num1=2.7;
double num2=8.1;
if(Math.abs(num1-num2)<1E-9)
```

**字符类型**
char ---- 两个字节(可以存放汉字)
使用细节
1.char本质是整数,是对应的unicode码`(int)c1`
2.可以直接给char赋值整数,然后输出是对应的unicode的字符
3.unicode装换工具:http://tool.chinaz.com/Tools/Unicode.aspx
4.char类型可以运算
5.字符编码
* ASCII一个字节表示,一个字节表示128个字符,理论上一个字节可以组合`2^8` 的字符,但是ASCII只用一个
* Unicode固定编码大小, 两个字节表示
* utf-8, 大小可变,字母用1个, 汉字用3个
* gbk , 可以表示汉字, 字母用1个, 汉字2个

**布尔类型**
## 基本数据类型的转换和String类型的转换
**基本数据类型的转换**
1.自动类型转换
![](assest/Pasted%20image%2020241105092911.png)
浮点数具有小数的表达能力, 自然比整数的精度高
细节
* 多种数据类型混合运算,系统先将所有数据转换成精度大的数据类型,然后计算
* [byte,short和char之间不会相互自动转换. 他们三者可以相互计算,在**计算**时首先转换成Int类型.](细节解释)
* boolean不参与转换
* 自动提升原则: 表达式结果的类型自动提升为操作数中最大的类型

2.强制类型转换
细节
* 当进行数据的精度大小从大-->小 , 就需要使用到强制类型转换
* char类型可以保存int的常量值,但不能保存int的变量值.(编译器类型检查机制和char的取值范围限制)
```java
char c1=100;
int m=100;
char c2=m;//错误
```
**JDK类的组织形式**
![](assest/Pasted%20image%2020241105091252.png)

**基本数据类型和String类型的转换**
1.基本介绍和使用
* 基本类型转String类型: 将基本数据类型+""即可
* String类型转基本数据类型: 通过基本类型的包装类调用parseXX方法,要保证是有效的整数, 如果格式错误,它会抛出异常
# 运算符
**关系运算符**
`instanceof`检查是否是类的对象比如 , `"string" instanceof String`

**逻辑运算符**
介绍: 用于连接多个条件, 最终的结果也是一个boolean值
(1)短路与`&&` , 短路或`||` , 取反`!`
(2)逻辑与`&` , 逻辑或`|` , 逻辑异或`^`
![](assest/Pasted%20image%2020241105123720.png)
短路和逻辑的区别 
* 短路: 当第一个条件为false是,第二个条件不用运算
* 逻辑: 即使第一个条件为false, 都要对第二个条件进行判断

**三元运算符**
基本语法: `条件表达式?表达式1:表达式2`
1.如果条件表达式为true，运算后的结果是表达式1；
2.如果条件表达式为false，运算后的结果是表达式2；

**关键字**
是在java程序中提前被保留下来的
![](assest/Pasted%20image%2020241105124905.png)
![](assest/Pasted%20image%2020241105124909.png)

**键盘输入语句**
1.Scanner类表示简单文本扫描器, 在new一个Scanner实例的时候要引入参数System.in
2.Scanner的常用方法
* next() 读取下一个单词(一空格或换行分隔符的字符串)
* `nextLine()`: 读取一整行, 直到行结束(包括空格)
* nextInt(): 读取下一个int类型整数值
* `nextDouble()`读取下一个双精度浮点数
* `close()`: 关闭`Scanner`对象,释放资源
# 数组,排序,查找
## 数组
**数组赋值机制**
1.基本数据类型赋值, 这个值就是具体的数据 , 相互不影响(值拷贝)
```java
int n1=2;
int n2=n1;
n2=80;//n1还是2, n2变为80
```
2.数组在默认情况下是引用传递, 赋的值是地址
![](assest/Pasted%20image%2020241105161817.png)
解释
上面是数组的浅拷贝, 是指在拷贝数组时，只拷贝了数组的引用，而不是数组中的实际数据。他们共享一个对象的引用
浅拷贝的方法
* 使用`System.arraycopy()` , `Arrays.copyOf()`, array2 = array1(直接赋值)

深拷贝
深拷贝（Deep Copy）是指在拷贝数组时，不仅拷贝了数组本身的内容，还递归地拷贝了数组中的每个对象，从而创建完全独立的副本
深拷贝的方法
* 手动遍历并创建每个元素的副本
* 使用序列化和反序列化
```java
public static <T> T deepCopy(T object) {  
    T copiedObject = null;  
    try {  
        // 将对象写入字节流  
        ByteArrayOutputStream byteOut = new ByteArrayOutputStream();  
        ObjectOutputStream out = new ObjectOutputStream(byteOut);  
        out.writeObject(object);  
        out.flush();  
  
        // 从字节流中读出对象  
        ByteArrayInputStream byteIn = new ByteArrayInputStream(byteOut.toByteArray());  
        ObjectInputStream in = new ObjectInputStream(byteIn);  
        copiedObject = (T) in.readObject();  
    } catch (IOException | ClassNotFoundException e) {  
        e.printStackTrace();  
    }  
    return copiedObject;  
}
```

**数组的扩容**
1.定义新数组`int[] ar=new int[arr.length+1]`, 然后深拷贝
# 面向对象基础
## 类和对象
**类和对象的关系示意图**
![](assest/Pasted%20image%2020241105165953.png)
![](assest/Pasted%20image%2020241105170006.png)
解释
* 类是抽象的, 代表着一类事务 , 比如人类, 计算机, 即他是数据类型
* 对象是具体的 , 实际的. 按照类的模版产生 , 比如 ,小明 , 猫
* 类是对象的模版, 对象是类的一个个个体

**对象在内存中存在的形式**
![](assest/Pasted%20image%2020241105170212.png)
类的组成成分: 字段,属性(任意数据类型)

**类和对象的内存分配机制**
1.java内存的结构分析
* 栈: 一般存放基本数据类型(局部变量)
* 堆: 存放对象(Cat cat , 数组等)
* 方法区: 常量池(常量,比如字符串) , 类加载信息
## 方法
**方法的调用机制原理**
![](assest/Pasted%20image%2020241105171218.png)

**方法的好处**
提高代码的复用性
可以将实现的细节封装起来 , 然后供其他用户来调用即可

**方法的递归调用**
递归就是方法自己调用自己. 
* 执行一个方法时, 就创建一个新的受保护的独立空间(栈空间)
* 方法的局部变量是独立的,不会受到影响
* 方法中的引用数据类型(如数组,对象), 就会共享该引用类型的数据
* 当一个方法执行完毕 , 或遇到return , 就会返回, 遵守谁调用谁 , 就将结果返回给谁, 同时该方法消失

**方法重载**
java允许同一个类中, 多个同名方法的存在 , 但要求 , 形参列表不一致
意义: 减轻了起名的麻烦和记名的麻烦

**可变参数**
定义: java允许将同一个类中多个同名同功能但**参数个数不同**的方法,封装成一个方法, 通过可变参数实现
基本语法
```
访问修饰符 返回类型 方法名(数据类型... 形参名){

}
```
细节以及注意事项
1.可变参数的实参可以为0个或任意多个
2.实参可以为数组, 可变参数本质就是数组, 可以以数组的形式遍历
3.可变参数和普通类型参数放在一起作为形参列表时, 必须保证可变参数在最后
4.==一个形参列表只能出现一个可变参数.==

**作用域**
介绍
1.局部变量: 方法中定义的变量, 作用域只在该代码块
2.全局变量: 类中直接定义的变量 , 也被称为属性. 作用域为整个类体. 可以加修饰符
3.全局变量可以不赋值, 直接使用 , 因为有默认值 , 局部变量必须赋值后才能使用, 因为没有默认值.
使用细节
1.属性和局部变量可以重名 , 访问时遵循就近原则
2.在同一个作用域中, 比如在同一个成员方法中, 两个局部变量不能重名
3.全局变量(属性)声明周期较长.伴随着对象的消亡. 局部变量伴随着代码块执行完毕而消亡
## this关键字
**介绍**
java虚拟机会给每个对象分配this, 代表当前对象 .

**this的使用细节**
1.this可以用来区分当前类的属性和局部变量
2.this不能在类定义的外部使用 , 只能在类定义的方法中使用
# 面向对象中级
## IDEA的常见快捷键
1.删除当前行, 默认是ctrl+Y 
2.复制当前行, 自己配置ctrl+alt+向下光标
3.补全代码 alt+/
4.添加注释和取消注释ctrl+/
5.导入该行需要的类 , alt+enter
6.快速格式化代码 ctrl+alt+L
7.快速运行代码 alt+r(自己定义)
8.生成构造器 , alt+insert
9.查看类的层级关系ctrl+h
10.将光标放在一个方法上，输入ctrl + B , 可以定位到方法
## 包,访问修饰符,面向对象编程特征
**包**
1.包的三大作用: 区分相同名字的类, 管理类 , 控制访问范围
2.包的基本语法:
* `package` 关键字,表示打包
* `com.ohmygod` 表示包名
3.包的命名规范
* 只能有数字,字母,下划线,圆点. 不能数字开头,不能关键字
* 一般是小写字母com.公司名.项目名.业务模块名

**访问修饰符**
![](assest/Pasted%20image%2020241107203628.png)
细节
* 只有默认和**public**才能修饰类

**面向对象编程三大特征**
封装,继承和多态
1.封装
* 把抽象出的数据和对数据的操作放在一起, 数据被保护在内部 , 程序的其它部分只有通过被授权的操作方法, 才能对数据进行操作
* 隐藏实现细节, 方法(连接数据库)< --调用(传入参数)
* 封装实现, 1.提供一个public set方法,用于属性修改, 提供public get用于属性访问

2.继承
技术背景: 比如写了两个业务相似度很高的代码, 有很多属性和方法相同的. 那么就可以通过继承实现代码复用
示意图
![](assest/Pasted%20image%2020241110170215.png)
子类能用父类的方法
继承的细节
* 子类继承所有的**属性**和方法, 非私有属性和方法可以在子类直接访问. 但是私有属性和方法不能在子类直接访问
* 子类必须调用父类的构造器 , 完成父类的初始化
* 当创建子类对象时, 不管使用子类的哪个构造器，默认情况下总会去调用父类的无参构造器，如果父类没有提供无参构造器，则必须在子类的构造器中用super 去指定使用父类的哪个构造器完成对父类的初始化工作, 否则编译失败
* super 在使用时，必须放在构造器第一行
* java所有的最终父类都是object()
子类创建的内存布局
![](assest/Pasted%20image%2020241110171541.png)
* 先加载父类 , jvm会先判断父类是否被加载. 这个过程会递归地向上追溯. 直到顶层的Object被加载
* 初始化父类的静态成员和静态代码. 
* 加载子类
* 初始化父类的实例成员和构造代码
* 初始化子类的实例成员和构造代码块

**super关键字**
![](assest/Pasted%20image%2020241110172244.png)
细节
* 调用父类构造器的好处(分工明确 , 父类属性由父类初始化, 子类属性由子类初始化)
* 子类和父类中成员重名时,需要super区分, 如果没重名 , super,this.直接访问效果一样
* super的访问不局限于直接父类. 如果多个基类都有同名成员. 使用super遵循就近原则.也遵循权限规则

super和this的比较
![](assest/Pasted%20image%2020241110173154.png)
## 方法重写以及Object类
**方法重写**
1.技术背景: 让子类有机会定义特定行为, **支持运行时多态(多态性的关键)**, 增强可维护性和可扩展性 , 遵循开闭原则

2.子类有个方法和父类的某个方法的名称 , 返回类型 , 参数一样, 这个子类就重写了父类的方法

3.细节
* 子类的方法的形参列表, 方法名称要和父类方法的形参列表, 方法名称完全一样.
* 子类方法的返回类型和父类方法返回类型一样 , 或者是父类返回类型的子类
* 子类不能缩小父类方法的访问权限

4.重写和重载的比较
![](assest/Pasted%20image%2020241110175016.png)
## 多态
**多态的介绍**
多态（Polymorphism）是指程序在编译时无法确定调用哪个方法，直到运行时根据对象的实际类型动态地决定执行哪个方法。

**多态的实现方式**
1.方法重载
同一个类中存在多个同名方法 , 但参数列表不同, 编译器在编译时选择最合适的重载方式.这属于编译时多态
2.方法重写
子类重写父类的同名方法 , 使得同一方法在不同的类中有不同的实现. 通过父类的引用调用重写的方法时，实际执行的是子类的方法，实现了运行时多态。

**方法的多态**
细节
* 一个对象的编译类型和运行类型可以不一致
* 编译类型在定义对象时, 就确定了, 不能改变
* 运行类型是可以变化的
* 编译类型是`=`号的左边 , 运行类型看`=`号的右边
```java
Animal animal = new Dog();
animal = new Cat();
```
animal编译类型是Animal , 运行类型是Dog
animal编译类型仍是Animal , 运行类型是Cat. 这种情况就解释了编译类型不能改变,运行类型可以改变的情景

**多态的转型**
1.向上转型
* 本质: 父类的引用指向了子类的对象
* 语法: 父类类型 引用名=new 子类类型();
* 特点: 编译类型看左边 , 运行类型看右边. 可以调用父类中所有成员(遵循访问权限), 不能调用子类中特有成员; 最终运行效果看子类的具体实现
2.多态向下转型
* 语法: 子类类型 引用名 = (子类类型) 父类引用
* 只能强转父类的引用 , 不能强转父类的对象
* 要求父类的引用必须指向的是当前目标类型的对象(转型后的对象)
* 向下转型后 , 可以调用子类类型中所有的成员

**多态的应用**
1.多态数组: 数组定义类型为父类类型 , 里面保存的实际元素类型为子类类型
2.多态参数: 方法定义的形参为父类类型 , 实参类型允许为子类类型
## Object类详解
**equals方法**
`==`和`equals`的对比
1.`==`既可以判断基本类型 , 又可以判断引用类型
* `==` 如果判断基本类型 , 判断的是值是否相等.
* `==`如果判断引用类型 , 判断的是地址是否相等 , 即判断是否同一个对象.
2.equals: 是Object类中的方法 , 只能判断引用类型. 默认判断的是地址是否相等, 子类中往往重写该方法 , 用于判断内容是否相等. 比如Integer ,String

**重写equals方法**
```java
@Override  
public boolean equals(Object anobject) {  
    if(this==anobject){  
        return true;  
    }  
    if(anobject instanceof B){  
        B b = (B)anobject;  
        if(this.b==b.b&&this.c==b.c){  
            return true;  
        }  
    }  
    return false;  
}
```

**hashCode方法**
1.介绍: 定义在 `Object` 类中的方法，返回对象的哈希值。主要用途是在哈希表等数据结构中高效地存储和查找对象，比如在 `HashMap`、`HashSet`、`Hashtable` 等集合中。

**toString方法**
1.介绍:
* 默认返回: 全类名+@+哈希值的16进制.
* 子类往往重写toString方法. 用于返回对象的属性信息
2.重写toString方法 , 打印对象或拼接对象时, 都会自动调用该对象的toString形式
3.当直接输出一个对象时 , toString方法会被默认的调用 , 比如`System.out.println(monster)`: 就会默认调用monster.toString()

**finalize方法**
1.当对象被回收时, 系统自动调用该对象的**finalize方法** . 子类可以**重写**该方法 , 做一些释放资源的操作
2.**什么时候被回收**. 当某个对象**没有任何引用**时, 则jvm就认为这个对象是一个垃圾对象, 就会使用垃圾回收机制来销毁这些对象, 在销毁对象前 , 会**先调用finalize**方法
3.垃圾回收机制的调用, 是由系统来决定(即有自己的GC 算法), 也可以通过System.gc() 主动触发垃圾回收机制
# 面向对象高级
## 类变量和方法
**类变量的定义**
类变量也叫静态变量, 是该类的所有对象共享的变量, 任何一个该类的对象去访问时, 取到的都是相同的值, 同样任何一个该类的对象去修改, 修改的也是同一个变量. 类变量是随着类加载而创建的.所以可以类名.类变量名这种方式去访问

**类方法**
形式如下
访问修饰符 static 数据返回类型 方法名() {}
static 访问修饰符 数据返回类型 方法名() {}

类方法的细节
* 静态属性都是随着类的加载而加载 , 将结构信息存储在方法区. 
* 类方法中无this的参数
* 类方法中不允许使用和对象有关的关键字, 比如this和super, 普通方法可以
* 类方法中只能访问静态变量或静态方法.
# 泛型
**泛型的技术背景**
去取出集合中的数据, 在对这些数据进行操作的时候, 操作不小心转型错误. 而且频繁的转型也会降低程序效率
```java
public class demo {  
    public static void main(String[] args){  
        ArrayList arrayList=new ArrayList();   
        arrayList.add(new A());  
        arrayList.add(new A());  
        arrayList.add(new B());  
        for(Object item : arrayList){  
            A a=(A)item;  
            System.out.println(a.a);  
        }  
    }  
}
```
泛型:
* 编译时, 检查添加元素的类型 , 提高了安全性
* 不使用泛型`arrayList.add(Dog)`, Dog->加入->Object->取出->Dog
* 使用泛型 Dog->Dog->Dog放入时和取出时, 不需要类型转换,提高效率
1.泛型又称为**参数化类型** 类似`E=Integer`, 是JDK5.0出现的新特性 , 解决数据类型安全性问题
2.在类声明或实例化时只要指定好需要的具体的类型即可
3.Java泛型可以保证如果程序在编译时没有发出警告, 运行时就不会产生ClassCastException异常
4.泛型的作用是:可以在类声明时通过一个标识表示类中某个属性的类型, 或者是某个方法的返回值的类型, 或者是参数类型. 类型的确定是在**编译期间**
```java
class Person<E>{
	E s;//该数据类型是在定义Person对象的时候指定的,即在编译期间
	public Person(E s){
		this.s=s;
	}
	public E f(){
		return s;
	}
}
```

**泛型的语法**
1.泛型声明
`interface 接口<T>{}`和`class 类<K,V,M>{}`//表示多个类型,泛型就是将数据类型参数化
T,K,V不代表值, 而是表示类型
2.泛型实例化
`List<String> strList=new ArrayList<String>{}`
`Iterator<Customer> iterator=customers.iterator();`

**泛型的细节**
1.参数化类型只能是引用类型
2.在给定泛型指定具体类型后, 可以传入该类型或者子类类型
3.在`List list3=new ArrayList()`默认给它的泛型是`<E> E就是Object`
```java
ArrayList<Integer> list1=new ArrayList<Integer>();//最完整的写法
List<Integer> list2=new ArrayList<Integer>();//用接口接收值,简写
List<Integer> list3=new ArrayList<>();//编译器会进行类型推断.推荐这种写法
```

**自定义泛型**
1.自定义泛型类
```java
class 类名 <T,R>{//..表示可以有多个泛型
	成员
}
```
注意事项
* 普通成员可以使用泛型(属性,方法)
* 使用泛型的数组, 不能初始化. 但能定义
* 静态方法中不能使用类的泛型, 因为泛型类的类型 , 是在创建对象时确定的
* 如果创建对象时, 没有指定类型 , 默认为Object

2.自定义泛型接口
```java
interface 接口名<T,R..>{
}
```
注意事项
* 接口中, 静态成员不能使用泛型
* 泛型接口的类型, 在继承接口或者实现接口时确定
* 没有指定类型, 默认为Object

3.自定义泛型方法
```java
修饰符 <T,R..> 返回类型 方法名(参数列表){
}
//<T,R>是泛型,提供给fly使用的
```
1.泛型方法, 可以定义在普通类中, 也可以定义在泛型类中
2.当泛型方法被调用时, 类型会确定
3.是为了在方法中处理多种类型的数据, 而且不牺牲安全性
# 多线程

## 线程的相关概念
**程序**
是为完成特定任务 , 用某种语言编写的一组指令的集合, 简单点说就是我们的代码,程序是静态的, 存储在磁盘上,程序本身不执行,只是一份文件或资源
**进程**
进程是指运行中的程序, 比如我们使用QQ, 就启动了一个进程, 操作系统就会为其分配内存空间. 当我们再启动一个游戏 , 就代表着又启动了一个进程 , 操作系统将为迅雷分配新的**内存空间**,进程是动态的,每个进程都有**独立地址空间**, 代码段, 数据段, 堆栈等
**线程**
线程由进程创建的, 是进程的一个实体, 一个进程可以拥有多个线程. 是**进程的一个执行流**. **线程之间共享进程的内存空间和资源**（如堆、全局变量等），但每个线程都有自己的栈和寄存器。
![](assest/{42694F5C-77A1-4275-8054-BC1D12A4204F}.png)
![](assest/Pasted%20image%2020240926204318.png)
## 线程的基本使用
1.**继承Thread**
1.继承`Thread`类,重写`run()`方法, 然后调用`start()`方法启动线程
```java
public class main {  
    public static void main(String[] args) {  
        CatThread catThread = new CatThread();  
        catThread.start();    
    }  
}  
class CatThread extends Thread{  
    public void run(){  
        for(int i=0;i<15;i++){  
            try {  
                Thread.sleep(2000);  
                System.out.println("猫猫数数"+i);  
            } catch (InterruptedException e) {  
                throw new RuntimeException(e);  
            }  
        }  
    }  
}  
```

![](assest/{2452B167-2D8D-43CA-9F12-4C0D213B3F95}.png)
细节 , `start()`才是开启真正的线程, 而不是调用普通的`run()`方法
![](assest/Pasted%20image%2020240927194805.png)

Runable接口的引出
java是单继承的, 在某些情况下一个类可能已经继承了某个父类, 这时在用继承Thread类方法来创建线程显然不适用. java设计者们提供了另一个方式创建线程, 就是通过实现Runnable接口创建线程
**2.实现`Runable`接口**
```java  
public class application {  
    public static void main(String[] args){  
        CatThread catThread = new CatThread();    
        Thread cat_thread = new Thread(catThread);   
        cat_thread.start();  
    }  
}
public class CatThread implements Runnable{  
    @Override  
    public void run() {  
        try {  
            for(int i=0;i<15;i++){  
                System.out.println("猫猫数数="+i);  
                Thread.sleep(1000);  
            }  
        } catch (InterruptedException e) {  
            throw new RuntimeException(e);  
        }  
    }  
}
```
jconsole的使用
**细节**
为什么不是调用`run()`方法, 而是调用`start()`方法
如果你在主线程中直接调用 `run()` 方法，就像调用普通方法一样，`run()` 方法会在当前线程（即主线程）中执行，不会创建一个新的线程去并发地执行它。**`start()` 方法的作用** 是 **启动一个新的线程，并让它在新的线程中执行 `run()` 方法**。当你调用 `start()` 方法时，Java 虚拟机（JVM）会为该线程分配资源，执行操作系统的**底层线程管理**，创建一个新的线程，然后在这个新的线程中自动调用 `run()` 方法。
![](assest/{AD2D654C-EA51-4F51-AFB1-1417D181FE6A}.png)
这个start方法是由jvm 调用的

**3.Thread和Runnable的区别**
1.从java的设计来看, 通过继承Thread或者实现Runnable接口来创建线程本质上是没有区别. 他们底层都是实现了Runnable接口
2.实现Runnable接口方式更加适合多个线程共享一个资源的情况, 并且避免了单继承的限制, 建议使用Runnable
## 线程的常用方法
1.setName //设置线程的名称
2.getName //返回该线程的名称
3.start //使该线程变为可运行态, java虚拟机底层调用该线程的start()方法
4.run //调用线程对象run方法
5.setPriority 更改线程的优先级
6.getPriority 获取线程的优先级
7.sleep 在指定的毫秒数内让当前正在执行的线程休眠
8.interrupt 中断线程
9.getState 获取线程当前状态
**注意细节**
1.start 底层会创建新的线程,调用run, run就是一个简单的方法调用, 不会启动新线程
2.线程优先级的范围
3 interrupt, 中断线程, 当并没有结束线程. 所以一般用于中断正在休眠线程
如果线程正处于某些 **阻塞状态**（例如 `sleep()`、`wait()` 或 `join()`），则 `interrupt()` 会让这些方法抛出 **`InterruptedException`**，使线程能够尽早从阻塞状态中退出。
4 sleep:线程的静态方法, 使当前线程休眠
![](assest/Pasted%20image%2020240929093725.png)
![](assest/Pasted%20image%2020240929093734.png)
## 线程的生命周期
**线程的状态**
JDK中用Thread.State枚举表示线程的几种状态
```java
public static enum Thread.State
extends Enum<Thread.State>
```
1.NEW:尚未启动的线程处于该状态,此线程还没有开始执行
* 调用start()方法后,线程进入Ready状态
2.Ready(就绪): 线程已经准备好等待cpu调度器分配CPU时间片进行运行,但没被调度
3.RUNNABLE(Running): 在java虚拟机中执行的线程处于该状态,线程获取了CPU时间片
* 线程可以调用 `Thread.yield()` 方法让出 CPU,进入Ready状态,或者线程执行完毕,进入Terminated状态
4.BLOCKED: 线程因为无法获取到同步锁（监视器锁）而处于阻塞状态，等待进入临界区。(进入同步块的情况下)
* 获得同步锁,线程进入Ready状态,等待CPU调用
5.WAITING: 等待状态，等待其他线程显式地唤醒它。
* 可以通过`Object.wait()`,`Thread.join()`,`LockSupport.park()`进入等待状态. 等待期间需要**其它线程**用`notify()`,`notifyAll()`或`LockSupport.unpark()`来唤醒线程,进入Ready状态.有些需要在[同步代码块中使用](细节解释)
6.TIMED_WAITING: 线程等待一定时间后自动苏醒，而不是无限期等待。
* 通过`Thread.sleep(time)`,`Object.wait(time)`,`Thread.join(time)`进入该状态
7.TERMINSTED:线程完成执行或因异常退出，进入终止状态。

**线程状态转换图**
![](assest/Pasted%20image%2020241108084840.png)

## 线程的同步
**1.基本介绍**
技术背景: 在多线程中 , 一些敏感的数据不允许被多个线程同时访问. 此时使用同步访问技术, 可以保证数据在任意时刻.最多只有一个线程访问(只有一个线程可以对内存操作). 保证数据的完整性
比如
```
- 线程A读取了变量的值为 10。
- 线程B读取了变量的值也为 10。
- 线程A对变量加 1 并写回，将变量的值更新为 11。
- 线程B对变量加 1 并写回，将变量的值更新为 11（覆盖了线程A的修改）。
- 最终结果会是 11，而预期值应该是 12。这种情况被称为线程安全问题。通过线程同步机制，可以避免这种并发问题。
```
定义: 线程的同步是一种用于控制多个线程访问共享资源的机制。

**2.同步的具体方法**
synchronized
声明在代码块中(同步代码块)
```java
synchronized(对象){//得到对象的锁,才能进行同步操作
	//需要同步的代码
}
```
放在方法声明中,表示整个方法-(同步方法)
```java
public synchronized void m(String name){
	//需要被同步的代码
}
```
厕所一人一厕所的问题, 一般是定义一个静态锁对象.然后所有线程使用这个锁对象

**3.同步的原理**
![](assest/Pasted%20image%2020241110090713.png)
线程同步的原理是利用**锁**机制来保证共享资源在多线程环境中的安全访问.

1.锁(锁是线程同步的关键工具)
* 每个对象都被一个可称为'互斥锁'的标记. 用来确保在任一时刻,只有一个线程可以访问对象. 持有锁的线程可以进入同步代码区. 其它线程等锁释放
* 锁的持有和释放. 第一个进入同步区的线程持锁. 等该线程退出同步区释放锁
* 同步的局限性. 导致程序的执行效率慢
* [同步方法(非静态)的锁可以是this(当前实例).也可以是其它对象(要求所有线程使用的相同的锁对象)](细节解释)
2.锁的实现
* synchronized关键字
* 实现Lock接口
* 要求多个线程的锁对象同一个即可

3.线程的死锁
定义: 线程死锁指的是两个或多个线程互相等待对方释放资源，导致程序永久阻塞的情况。
死锁的经典场景
```java
public class DeadlockExample {
    private static final Object lock1 = new Object();
    private static final Object lock2 = new Object();

    public static void main(String[] args) {
        Thread thread1 = new Thread(() -> {
            synchronized (lock1) {
                System.out.println("Thread 1: Holding lock1...");
                try { Thread.sleep(100); } catch (InterruptedException e) {}
                System.out.println("Thread 1: Waiting for lock2...");
                synchronized (lock2) {
                    System.out.println("Thread 1: Acquired lock2!");
                }
            }
        });
        Thread thread2 = new Thread(() -> {
            synchronized (lock2) {
                System.out.println("Thread 2: Holding lock2...");
                try { Thread.sleep(100); } catch (InterruptedException e) {}
                System.out.println("Thread 2: Waiting for lock1...");
                synchronized (lock1) {
                    System.out.println("Thread 2: Acquired lock1!");
                }
            }
        });
        thread1.start();
        thread2.start();
    }
}
```

4.锁的释放
* 当前线程的同步方法,同步代码块执行结束
* 当前线程在同步代码块和同步方法中遇到break,return
* 当前线程在同步代码块和同步方法遇到未处理的Error或Exception.
* 当前线程在同步代码块和方法中执行了线程对象的wait()方法,当前线程暂停,并释放锁
* 线程执行同步代码块或同步方法时, 程序调用Thread.sleep(), Thread.yield()方法暂停当前线程的执行, 不会释放锁
* 线程执行同步代码块时, 其它线程调用了该线程的suspend()方法将该线程挂起. 该线程不会释放锁
# 流与文件
![](Java高级/assest/Pasted%20image%2020240913190845.png)
## 流
**1.1流**
输入流: 能够读字节序列的对象
输出流: 能够输出字节序列的对象
字节流: `InputStream`和`OutputStream`构成有层次结构的输入/输出(I/O)类的基础
字符流: [面向字节的流不便于处理Unicode形式]([](字符流)), 所以从抽象类`Reader`和`Writer`中继承出来专门处理Unicode字符的类单独构成一个独立的层次结构. 这些类拥有的读入和写出操作都是基于两字节的Unicode码元.
**`read`** 和 **`write`** 方法在进行输入输出操作时，如果没有立即获得所需的数据（如在读取数据时流中暂时没有数据，或者在写入数据时目标设备未准备好），当前线程会**暂停**，直到数据准备好，操作才能继续。这段暂停的时间称为“阻塞”，它使得当前线程无法继续执行，直到完成输入或输出操作。
