# Java 接口

### 接口特性

* 接口中每一个方法是隐式抽象的,接口中的方法会被**隐式**的指定为 **public abstract**（只能是 public abstract，其他修饰符都会报错）。
* 接口中可以含有变量，但是接口中的变量会被隐式的指定为 **public static final** 变量（并且只能是 public，用 private 修饰会报编译错误）。
* 一个类可以同时实现多个接口
* 一个接口能继承另一个接口，这和类之间的继承比较相似
* 接口允许多继承
* 当类实现接口的时候，类要实现接口中所有的方法。否则，类必须声明为**抽象类**。

### 接口的声明

接口的声明语法格式如下：

```
[可见度] interface 接口名称 [extends 其他一或多个接口名] {
        // 声明变量
        // 抽象方法
}
```

重写接口中声明的方法时，需要注意以下规则：

* 类在实现接口的方法时，不能抛出强制性异常，只能在接口中，或者继承接口的抽象类中抛出该强制性异常。
* 如果实现接口的类是抽象类，那么就没必要实现该接口的方法。

### 标记接口

最常用的继承接口是没有包含任何方法的接口。

标记接口是没有任何方法和属性的接口。它仅仅表明它的类属于一个特定的类型,供其他代码来测试允许做一些事情。

标记接口作用：简单形象的说就是给某个对象打个标（盖个戳），使对象拥有某个或某些特权。

例如：`java.awt.event` 包中的 `MouseListener` 接口继承的 `java.util.EventListener` 接口定义如下：

```java
package java.util; 
public interface EventListener {
}
```

## Java8新特性：接口的默认方法和静态方法

### 接口增强

Java 8 对接口做了进一步的增强。

**a.** 在接口中可以添加使用 default 关键字修饰的非抽象方法。即：默认方法（或扩展方法）

**b.** 接口里可以声明静态方法，并且可以实现。

### 默认方法

>  Java 8 允许给接口添加一个非抽象的方法实现，只需要使用 `default`关键字即可，这个特征又叫做扩展方法（也称为默认方法）。在实现该接口时，**该默认扩展方法在子类上可以直接使用**，它的使用方式类似于抽象类中非抽象成员方法。

Note：扩展方法不能够重写`Object类` 中的方法，却可以重载`Object类`中的方法。

eg：toString、equals、 hashCode 不能在接口中被覆盖，却可以被重载。

默认方法允许我们在接口里添加新的方法，而不会破坏实现这个接口的已有类的兼容性，也就是说不会强迫实现接口的类实现默认方法。

默认方法和抽象方法的区别是抽象方法必须要被实现，默认方法不是。作为替代方式，接口可以提供一个默认的方法实现，所有这个接口的实现类都会通过继承得到这个方法（如果有需要也可以重写这个方法）

```java
interface Defaulable {
    //使用default关键字声明了一个默认方法
     @SuppressLint("NewApi")
     default String myDefalutMethod() {
        return "Default implementation";
    }
}
class DefaultableImpl implements Defaulable {
    //DefaultableImpl实现了Defaulable接口，没有对默认方法做任何修改
}
class OverridableImpl implements Defaulable {
        //OverridableImpl实现了Defaulable接口重写接口的默认实现，提供了自己的实现方法。
        @Override
        public String myDefalutMethod() {
            return "Overridden implementation";
        }
```

### 静态方法

接口里可以声明静态方法，并且可以实现。

````java

private interface DefaulableFactory {
   // Interfaces now allow static methods
   static Defaulable create(Supplier< Defaulable > supplier ) {
       return supplier.get();
   }
}
````



### 参考资料

1. 菜鸟教程---Java接口 https://www.runoob.com/java/java-interfaces.html
2.  Java8新特性：接口的默认方法和静态方法  https://blog.csdn.net/sun_promise/article/details/51220518