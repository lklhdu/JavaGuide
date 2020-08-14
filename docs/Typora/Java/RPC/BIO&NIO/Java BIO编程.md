# Java BIO编程

## 基本说明

Java BIO ： 同步并且阻塞。服务器实现模式为**一个连接一个线程**，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销

![image-20200807091331934](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200807094511-247325.png)

## BIO应用实例

代码路径为`‪F:\nettyLearning\src\main\java\bio\BioServer.java`

```java
public class BIOServer {
    public static void main(String[] args) throws Exception {

        //线程池机制

        //思路
        //1. 创建一个线程池
        //2. 如果有客户端连接，就创建一个线程，与之通讯(单独写一个方法)

        ExecutorService newCachedThreadPool = Executors.newCachedThreadPool();

        //创建ServerSocket
        ServerSocket serverSocket = new ServerSocket(6666);

        System.out.println("服务器启动了");

        while (true) {
            
            //监听，等待客户端连接
            System.out.println("等待连接....");
            final Socket socket = serverSocket.accept();
            System.out.println("连接到一个客户端");

            //就创建一个线程，与之通讯(单独写一个方法)
            newCachedThreadPool.execute(new Runnable() {
                public void run() { //我们重写
                    //可以和客户端通讯
                    handler(socket);
                }
            });
        }
    }

    //编写一个handler方法，和客户端通讯
    public static void handler(Socket socket) {

        try {
            System.out.println("线程信息 id =" + Thread.currentThread().getId() + " 名字=" + Thread.currentThread().getName());
            byte[] bytes = new byte[1024];
            //通过socket 获取输入流
            InputStream inputStream = socket.getInputStream();

            //循环的读取客户端发送的数据
            while (true) {

                System.out.println("线程信息 id =" + Thread.currentThread().getId() + " 名字=" + Thread.currentThread().getName());

                System.out.println("read....");
                int read =  inputStream.read(bytes);
                if(read != -1) {
                    System.out.println(new String(bytes, 0, read
                                                 )); //输出客户端发送的数据
                } else {
                    break;
                }
            }
            
        }catch (Exception e) {
            e.printStackTrace();
        }finally {
            System.out.println("关闭和client的连接");
            try {
                socket.close();
            }catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```

### 相关测试

`telnet`方式

```
cmd+R 运行命令行窗口
telnet 127.0.0.1 6666
ctrl+ ] 进入Escape
send [message]
执行quit 完全退出
可以开多个命令行窗口模拟多个客户端
```

### 问题分析

1. 通过添加的线程打印信息可以得知，连接建立后，如果当前线程暂时没有数据可读，则线程就**阻塞在read 操作上**
1. 当并发数较大时，需要创建大量线程来处理连接，系统资源占用较大。

### 参考资料

尚硅谷韩顺平老师的netty课程