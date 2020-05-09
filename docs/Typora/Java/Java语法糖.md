# Java语法糖

原文链接 https://hollischuang.github.io/toBeTopJavaer/#/basics/java-basic/syntactic-sugar

语法糖（Syntactic Sugar），指在计算机语言中添加的某种语法，这种语法对语言的功能并没有影响，但是更方便程序员使用。**简而言之，语法糖让程序更加简洁，有更高的可读性。**

## 1.switch支持String和Enum

```java
public class switchDemoString {
    public static void main(String[] args) {
        String str = "world";
        switch (str) {
            case "hello":
                System.out.println("hello");
                break;
            case "world":
                System.out.println("world");
                break;
            default:
                break;
        }
    }
}
```

## 2.泛型

## 3.自动装箱和拆箱

自动装箱：Java自动将原始类型值转换成对应的对象，比如将int的变量转换成Integer对象

拆箱：将对象转换成原始的类型值

**装箱过程是通过调用包装器的valueOf方法实现的，而拆箱过程是通过调用包装器的 xxxValue方法实现的。**

## 4.方法的可变参数

```java
public static void print(String... strs)
{
    for (int i = 0; i < strs.length; i++)
    {
        System.out.println(strs[i]);
    }
}
```

## 5.枚举

## 6.内部类

内部类又称为嵌套类，可以把内部类理解为外部类的一个普通成员。

## 7.条件编译

有时候出于对程序代码优化的考虑，希望只对其中一部分内容进行编译，此时就需要在程序中加上条件，让编译器只对满足条件的代码进行编译，将不满足条件的代码舍弃

## 8.断言

## 9.数值字面量

在java 7中，数值字面量，不管是整数还是浮点数，都允许在数字之间插入任意多个下划线。这些下划线不会对字面量的数值产生影响，目的就是方便阅读。

```java
public class Test {
    public static void main(String... args) {
        int i = 10_000;
        System.out.println(i);
    }
}
```

## 10.增强for循环（for-each）

## 11.try-with-resource

关闭资源的常用方式就是在`finally`块里是释放，即调用`close`方法

现在可以使用`try-with-resources`语句来关闭文件操作IO流等资源

```java
public static void main(String... args) {
    try (BufferedReader br = new BufferedReader(new FileReader("d:\\ hollischuang.xml"))) {
        String line;
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }
    } catch (IOException e) {
        // handle exception
    }
}
```



## 12.Lambda表达式