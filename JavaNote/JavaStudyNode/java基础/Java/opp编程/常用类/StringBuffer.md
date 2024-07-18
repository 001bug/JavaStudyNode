**基本介绍**
1. java.lang.StringBuffer代表可变的字符序列,可以对字符串内容进行增删
2. 很多方法和String相同, 但StringBuffer是可变长度的
3. StringBuffer是一个容器

**String和StringBuffer**
1. String保存的是字符串常量, 里面的值不能更改, 每次String类的更新实际上就是更改地址, 效率较低
2. StringBuffer保存的是字符串变量, 里面的值可以更改,每次StringBuffer的更新实际上可以更新内容, 不用每次更新地址,效率较高//char[] value, 这个放在堆里

**StringBuffer的构造器**
1. StringBuffer()---构造一个其中不带字符的[字符串缓冲区],其初始容量为16个字符
2. StringBuffer(CharSequence seq)
		public java.lang.StringBuilder(CharSequence seq)构造
	1个字符串缓冲区, 它包含与指定的CharSequence相同的字符
3. StringBuffer(int capacity)//capacity[容量]
	 构造一个不带字符,但具有指定初始容量的字符串缓冲区,即对char[]大小进行指定
4. StringBuffer(String str)
	 构造一个字符串缓冲区, 并将其内容初始化为指定的字符串内容

**StringBuffer和String的相互转换**
1. 使用构造器
2. 使用方法
```
package com.xinxin.String01;  
  
public class StringBuffer_String {  
    public static void main(String[] args){  
        //-----------//String--->StringBuffer  
        String str="hello";  
        //注意: 返回的才是StringBuffer对象, 对str本身没用影响  
        StringBuffer stringBuffer = new StringBuffer(str);//方式1  
        StringBuffer stringBuffer1 = new StringBuffer();//方式2使用append方法  
        StringBuffer append=stringBuffer1.append(str);  
        //-----------//StringBuffer--->String  
        StringBuffer stringBuffer3 =new StringBuffer("hello");  
        //方式1,调用StringBuffer中的toString()方法  
        String s = stringBuffer3.toString();  
        //方法二,使用String构造器  
        String s1 = new String(stringBuffer3);  
    }  
}
```

**StringBuffer类的常用方法**
1. 增 append
2. 删 delete(start,end)
3. 该 replace(start,end,string)//将start---end间的内容替换掉,不含end//类似于C++中容器迭代的左闭右开
4. 查 indexOf//查找子串在字符串第1次出现的索引,如果找不到返回-1
5. 插insert
6. 获取长度length
[在idea中点击dia中diagram再点左上方method](IDE)