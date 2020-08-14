# NIO与零拷贝

## 基本介绍

1. 零拷贝是网络编程性能优化的关键
1. Java中常用的零拷贝有`mmap`和`sendFile`

### 零拷贝

NIO并不能解决网络传输的速度，而NIO的速度比BIO快的原因就在于zero copy

零拷贝是从操作系统角度出发的，**是指CPU拷贝的次数为零**

零拷贝不仅仅带来更少的数据复制，还能带来其他的性能优势，例如更少的上下文切换，更少的 CPU 缓存伪共享以及无 CPU 校验和计算。

## 底层原理

**当希望将磁盘中的数据通过网络发送的时候**

### 传统IO

![image-20200813084856792](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200813084858-918723.png)

写部分是指将数据复制到内核态的socket buffer,然后才能发送数据

用户空间和内核空间的两次内存拷贝是“没有意义的”。当操作达到一定次数后，内存拷贝的耗时会导致速率的降低

### Linux2.4前的NIO

![image-20200813091553685](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200813091555-663518.png)

要想实现真正意义上的零拷贝，还是需要操作系统的支持

### Linux2.4后的NIO

![image-20200813091847692](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200813091849-848498.png)

![image-20200813091956371](C:\Users\lekelei\AppData\Roaming\Typora\typora-user-images\image-20200813091956371.png)

第二张流程图解释的会更清晰些，NIO只发生了两次用户态和内核态之间的上下文切换

Linux2.4之前：首先将硬盘（hard drive）上的数据复制到内核态缓存中（kemel buffer）。然后发生了一次拷贝（CPU copy）到socket缓存中(socket buffer)。最后再通过协议引擎将数据发送出去。

Linux2.4之后：实现了真正的零拷贝——上图中的从内核态缓存中（kemel buffer）的拷贝到socket缓存中(socket buffer)的就不再是数据了。而是**对内核态缓存中数据的描述符（也就是指针）**。协议引擎发送数据的时候其实是通过socket缓存中的描述符。找到了内核态缓存中的数据。再将数据发送出去。

## BIO、NIO、AIO对比

最后附上BIO、NIO、AIO的对比表

| ** **    | **BIO**  | **NIO**                | **AIO**    |
| -------- | -------- | ---------------------- | ---------- |
| IO 模型  | 同步阻塞 | 同步非阻塞（多路复用） | 异步非阻塞 |
| 编程难度 | 简单     | 复杂                   | 复杂       |
| 可靠性   | 差       | 好                     | 好         |
| 吞吐量   | 低       | 高                     | 高         |