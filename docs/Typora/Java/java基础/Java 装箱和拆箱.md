# Java 装箱和拆箱

### 装箱和拆箱的概念

Java为每种基本数据类型都提供了对应的包装器类型

| 基本类型        | 包装器类  |
| --------------- | --------- |
| int（4字节）    | Integer   |
| byte（1字节）   | Byte      |
| short（2字节）  | Short     |
| long（8字节）   | Long      |
| float（4字节）  | Float     |
| double（8字节） | Double    |
| char（2字节）   | Character |
| boolean（未定） | Boolean   |

装箱：自动根据数值创建对应的包装器类型对象

拆箱：自动将包装器类型转换为基本数据类型

```java
Integer i = 10;  //装箱
int n = i;   //拆箱
```

### 为基本类型提供包装器类型的原因

基本类型并不具有对象的性质，为了让基本类型也具有对象的特征，就出现了包装类型（如我们在使用集合类型Collection时就一定要使用包装类型而非基本类型），它相当于将基本类型“包装起来”，使得它具有了对象的性质，并且为其添加了属性和方法，丰富了基本类型的操作。

另外，当需要往ArrayList，HashMap中放东西时，像int，double这种基本类型是放不进去的，因为容器都是装object的，这是就需要这些基本类型的包装器类了。

### 装箱和拆箱的实现

以Integer类为例

在装箱的时候自动调用的是`Integer.valueOf(int)`方法，而在拆箱的时候自动调用的是`Integer.intValue()`方法。

### 面试中的一些问题

该篇博客中总结的很好 [深入剖析Java中的装箱和拆箱](https://www.cnblogs.com/dolphin0520/p/3780005.html)

一个小结论：Integer、Short、Byte、Character、Long这几个类的valueOf方法的实现是类似的。而Double、Float的valueOf方法的实现是类似的。