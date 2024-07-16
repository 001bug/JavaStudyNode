**基本介绍**
1. 一个可变的字符序列. 此类提供 一个与StringBuffer兼容的API,但不保证同步(StringBuilder不是[线程安全]), 该类被设计用作StringBuffer的一个简易替换,用在字符串缓冲区被[单个线程]使用的时候. 如果可能, 建议优先采用该类, 因为在大多数现实中, 它比StringBuffer要快
2. 在StringBuilder上的主要操作是append和insert方法, 可重载这些方法以接受任意类型的数据

**StirngBuilder源码分析**
![[Pasted image 20240227112544.png]]
1. StringBuilder继承AbstractStringBuilder类,方法和StringBuffer差不多
2. 实现了Serializable, 说明StringBuilder对象是可以串行化(对象可以网络传输,可以保存到文件)
3. StringBuilder是final类,不能被继承
4. StringBuilder对象字符序列仍然时存放在其父类AbstractStringBuilder的 char[] value; ,因此, 字符序列在堆中
5. StringBuilder的方法, 没有做互斥的处理, 即没有synchronized关键字,因此在单线程的情况下StringBuilder

![[Pasted image 20240227114352.png]]

**String,StringBuffer和StringBuilder的选择**
1. 如果字符串存在大量的修改操作, 一般使用StringBuffer或StringBuilder
2. 如果字符串存在大量的修改操作, 并在单线程的情况,使用 StringBuilder
3. 如果字符串存在大量的修改操作,并在多线程的情况,使用StringBuffer
4. 如果我们字符串很少修改,被多个对象引用,使用String , 比如配置信息等