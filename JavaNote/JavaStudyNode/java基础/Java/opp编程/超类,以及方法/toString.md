[[继承]]  [[IDE]]
toString 源码
public String toString() {  
    return getClass().getName() + "@" + Integer.toHexString(hashCode());  
}
返回getClass().getName()这个表示全类名(包名＋类名)  
Integer.toHexString(hashCode());//表示将hashCode值转变为16进制字符串

object中的方法toString方法
基本介绍: 
1.默认返回:全类名+@+哈希值的十六进制,  [查看Object的toString方法]子类往往重写toString方法,用于返回对象的[属性信息]

2.重写toString方法,打印对象或拼接对象时, 都会自动调用该对象的toString形式.

3.当直接输出一个对象时, toString 方法会被默认的调用,比如System.out.println(monster); 就会默认调用 monster.soString()

它的意义就是==以字符串的形式==表示类的属性

