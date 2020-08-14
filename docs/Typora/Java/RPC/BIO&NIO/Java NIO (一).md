# Java NIO (一)

## 基本介绍

1. Java NIO 全称**java non-blocking IO**，是同步非阻塞的
1. NIO 有三大核心部分：**Channel(通道)，Buffer(缓冲区), Selector(选择器)**
1. 实现模式：一个线程处理多个请求(连接)，即客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有I/O 请求就进行处理

![image-20200807110555674](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200807110557-74170.png)

## NIO和BIO的比较

1. BIO 以流的方式处理数据，而NIO以块的方式处理数据,块I/O 的效率比流I/O 高很多

   NIO 是**面向缓冲区，或者面向块编程的**。数据会读取到一个它稍后处理的缓冲区。缓冲区中内是可以前后移动的，这就增加了处理过程中的灵活性

1. BIO 是阻塞的，NIO 则是非阻塞的

   Java NIO 的非阻塞模式，使一个线程从某通道发送请求或者读取数据，会去获得目前可用的数据。如果目前没有数据可用时，该线程可以继续做其他的事情。而不是保持线程阻塞，直至有可以读取的数据

3. BIO 基于字节流和字符流进行操作，而NIO 基于Channel(通道)和Buffer(缓冲区)进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中。Selector(选择器)用于监听多个通道的事件（比如：连接请求，数据到达等），因此使用单个线程就可以监听多个客户端通道

   ![image-20200812214610563](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200812214612-982327.png)

<center>Selector，Channel，Buffer的关系图