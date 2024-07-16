**String类的理解和创建对象**
1. String 对象用于保存字符串, 也就是一组字符序列
2. 字符串常量对象是用双引号括起来的字符序列
3. 字符串的字符使用Unicode字符编码, 一个字符(不区分字幕还是汉字)占两个字节
4. String类较常用[构造器]
	1. String s1=new String();
	2. String s2 = new String(String original);
	3. String s3 = new String(char[] a);
	4. String s4 = new String(char[] a,int startIndex,int count);
	5. String s5 = new String(byte[] b);
5. String 有属性 private final char value[];用于存放字符串内容,注意,value是一个final类型,不可以修改指value指向的地址不能修改
![[Pasted image 20240226133202.png]]
1. String类实现了Serializable意味着String对象可以进行串行化,意味着String对象可以在网络中传输
2. 实现了Comparable接口,说明可以String对象可以比较


**Stirng创建方式剖析**
1. 方式一: 直接赋值String s="xinxin";
2. 方式二: 调用构造器String s = new String("xinxin");

方式一: 先从常量池查看是否有"xinxin"数据空间,如果有, 直接指向: 如果没有则重新创建, 然后指向. 最终指向的是常量池的空间地址
方式二: 先在堆中创建空间, 里面维护了value属性, 指向常量池的xinxin空间.如果常量池没有"xinxin", 重新创建, 如果有, 直接通过value指向. 最终指向的是堆中的空间地址

两种方式的内存分布图
![[Pasted image 20240226140929.png]]

String类常见方法
1. equals//区分大小写,判断内容是否相等
2. equalslanoreCase//忽略大小写的判断内容是否相等
3. length//获取字符的个数, 字符串的长度
4. indexOf//获取字符在字符串中第一次出现的索引,索引从0开始,如果找不到,返回-1
5. lastIndexOf//获取字符在字符串中最后一次出现的索引,索引从0开始,如找不到,返回-1
6. substring//截取指定范围的子串
7. trim//去前后空格
8. charAt:获取某索引处的字符,注意不能使用Str[index]这种方式

[分枝1](StringBuilder.md)[分枝2](StringBuffer.md)