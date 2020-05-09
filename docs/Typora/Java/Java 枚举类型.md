# java--枚举类型

以下内容参考自《疯狂Java讲义》

## 定义枚举类&简单使用

```java
public enum  SeasonEnum {
    //枚举类的所有实例必须在第一行显式列出
    SPRING,SUMMER,FALL,WINTER;
}
```

如果需要使用该枚举类的某个实例，可以使用`EnumClass.variable`的形式

枚举类默认有一个`values()`方法，可以返回该枚举类所有的实例

```java
public class EnumTest {
    public void judge(SeasonEnum s){
        //switch表达式可以使用Enum对象作为表达式
        switch (s){
            case SPRING:
                System.out.println("春暖花开");
                break;
            case SUMMER:
                System.out.println("夏日炎炎");
                break;
            case FALL:
                System.out.println("秋高气爽");
                break;
            case WINTER:
                System.out.println("冬日雪飘");
                break;
        }
    }

    public static void main(String[] args) {
        //使用values()方法返回该枚举类的所有实例
        for (SeasonEnum s:SeasonEnum.values()){
            System.out.println(s);
        }
        //使用枚举实例时，可以通过EnumClass.variable的形式来访问
        new EnumTest().judge(SeasonEnum.SUMMER);
    }
}
```

所以枚举类都继承了`java.lang.Enum`类，而`java.lang.Enum`类提供了下面几个方法

- `int compareTo(E e)`：该方法用于与指定枚举对象比较顺序
- `String toString()`：返回枚举常量的名称
- `int ordinal()`：返回枚举值在枚举类中的索引值

## 枚举类的成员变量、方法和构造器

枚举类是一种特殊的类，所以它一样可以定义成员变量、方法和构造器

枚举类通常应该设计成不可变类，也就是**它的成员变量值不应该允许改变**。因此建议将枚举类的成员变量使用`private final`修饰。

```java
/**
 *定义一个Gender枚举类
 */
public enum Gender{
    //此处的枚举值必须调用对应的构造器来创建
    MALE("男"),FEMALE("女");
    private final String name;
    //枚举类的构造器只能使用private修饰
    private Gender(String name){
        this.name=name;
    }
    public String getName(){
        return this.name;
    }
}
```

可以通过`Enum`的`valueOf()`方法来获取指定枚举类的枚举值

```java
public class GenderTest {
    public static void main(String[] args) {
        Gender g = Enum.valueOf(Gender.class, "FEMALE");
        System.out.println(g + " " + g.getName());
    }
}
//打印得到 FEMALE 女
```

## 实现接口的枚举类

枚举类和普通类一样可以实现一个或多个接口

```java
public interface GenderDesc{
    void info();
}
```

如果需要每个枚举值在调用该方法时呈现不同的行为方式，则可以让每个枚举值分别来实现该方法

```java
public enum Gender implements GenderDesc{
    MALE("男"){
        public void info(){
            System.out.println("这个枚举值代表男性");
        }
    },
    FEMALE("女"){
        public void info(){
            System.out.println("这个枚举值代表女性");
        }
    };
    //其余部分与上一小节的Gender类相同
    private final String name;
    //枚举类的构造器只能使用private修饰
    private Gender(String name){
        this.name=name;
    }
    public String getName(){
        return this.name;
    }
}
```

