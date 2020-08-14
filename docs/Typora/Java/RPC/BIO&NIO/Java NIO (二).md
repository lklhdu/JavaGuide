# Java NIO (二)

## 缓冲区Buffer

本质上是一个可以读写数据的内存块，可以理解成是一个容器对象(含数组)。并且提供了一组方法，可以更轻松地使用内存块

顶层父类`Buffer`,常用子类有`ByteBuffer`,`IntBuffer`,`CharBuffer`等

**属性**

![image-20200812221752859](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200812221758-411090.png)

**方法**

```java
/*ByteBuffer类的主要方法*/
public abstract class ByteBuffer {
    //缓冲区创建相关api
    public static ByteBuffer allocateDirect(int capacity)//创建直接缓冲区
    public static ByteBuffer allocate(int capacity)//设置缓冲区的初始容量
    public static ByteBuffer wrap(byte[] array)//把一个数组放到缓冲区中使用
    //构造初始化位置offset和上界length的缓冲区
    public static ByteBuffer wrap(byte[] array,int offset, int length)
     //缓存区存取相关API
    public abstract byte get( );//从当前位置position上get，get之后，position会自动+1
    public abstract byte get (int index);//从绝对位置get
    public abstract ByteBuffer put (byte b);//从当前位置上添加，put之后，position会自动+1
    public abstract ByteBuffer put (int index, byte b);//从绝对位置上put
 }
```

## 通道Channel

**NIO中的通道类似于IO的流**，但也有区别：通道既能读，也能写；通道可以异步读写；通道与缓冲区之间也是双向的

常 用 的 Channel 类 有 ： FileChannel 、 DatagramChannel 、 ServerSocketChannel 和
SocketChannel 。

**FileChannel 用于文件的数据读写**， DatagramChannel 用于UDP 的数据读写， **ServerSocketChannel 和SocketChannel 用于TCP 的数据读写。**

### 实例：使用FileChannel对本地文件进行读写操作

![image-20200812222718008](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200812222719-103265.png)

## 选择器Selector

Java 的NIO使用一个线程来处理多个的客户端连接，就会使用到Selector(选择器)

Selector 能够检测多个注册的通道上是否有事件发生(注意:多个Channel 以事件的方式可以注册到同一个Selector)。如果有事件发生，便获取事件然后针对每个事件进行相应的处理。

![image-20200812223100406](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200812223101-835651.png)

<center>Selector示意图

### 不足

NIO网络编程的示例没有编写（Server端和Client端），SelectionKey相关的也没有看