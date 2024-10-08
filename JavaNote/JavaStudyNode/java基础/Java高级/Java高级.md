# 并发

# 流与文件
![](Java高级/assest/Pasted%20image%2020240913190845.png)
## 流
**1.1流**
输入流: 能够读字节序列的对象
输出流: 能够输出字节序列的对象
字节流: `InputStream`和`OutputStream`构成有层次结构的输入/输出(I/O)类的基础
字符流: [面向字节的流不便于处理Unicode形式]([](字符流)), 所以从抽象类`Reader`和`Writer`中继承出来专门处理Unicode字符的类单独构成一个独立的层次结构. 这些类拥有的读入和写出操作都是基于两字节的Unicode码元.
**`read`** 和 **`write`** 方法在进行输入输出操作时，如果没有立即获得所需的数据（如在读取数据时流中暂时没有数据，或者在写入数据时目标设备未准备好），当前线程会**暂停**，直到数据准备好，操作才能继续。这段暂停的时间称为“阻塞”，它使得当前线程无法继续执行，直到完成输入或输出操作。
