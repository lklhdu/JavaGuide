# if-else的优化

原文地址：https://www.tuicool.com/wx/2euqQvZ

## 1.使用 return

我们使用 `return` 去掉多余的 else，实现代码如下。

优化前代码：

```java
if(condition){  
    //doSomething 
}else{  
    return ;  
} 
```

优化后代码：

```java
if（!condition）{  
    return ;  
}  
//doSomething 
```

这样看起来就会舒服很多

## 2.使用 Map

使用 Map 数组，把相关的判断信息，定义为元素信息可以直接避免 if else 判断，实现代码如下。

优化前代码：

```java
if (t == 1) {
    type = "name";
} else if (t == 2) {
    type = "id";
} else if (t == 3) {
    type = "mobile";
}
```

我们先定义一个 Map 数组，把相关判断信息存储起来：

```java
Map<Integer, String> typeMap = new HashMap<>();
typeMap.put(1, "name");
typeMap.put(2, "id");
typeMap.put(3, "mobile");
```

之前的判断语句可以使用以下一行代码代替了：

```java
type = typeMap.get(ty);
```

## 3.使用三元运算符

优化前代码：

```java
Integer score = 81;
if (score > 80) {
    score = 100;
} else {
    score = 60;
}
```

优化后代码：

```java
score = score > 80 ? 100 : 60;
```

## 4.使用枚举

JDK 1.5 中引入了新的类型——枚举（enum），我们使用它可以完成很多功能，例如下面这个。

优化前代码：

```java
Integer typeId = 0;
String type = "Name";
if ("Name".equals(type)) {
    typeId = 1;
} else if ("Age".equals(type)) {
    typeId = 2;
} else if ("Address".equals(type)) {
    typeId = 3;
}
```

优化时，我们先来定义一个枚举：

```java
public enum TypeEnum {
    Name(1), Age(2), Address(3);
    public Integer typeId;
    TypeEnum(Integer typeId) {
        this.typeId = typeId;
    }
}
```

之前的 if else 判断就可以被如下一行代码所替代了：

```java
typeId = TypeEnum.valueOf("Name").typeId;
```

## 5.使用 Optional

从 JDK 1.8 开始引入 Optional 类，在 JDK 9 时对 Optional 类进行了改进，增加了 ifPresentOrElse() 方法，我们可以借助它，来消除 if else 的判断，使用如下。

优化前代码：

```java
String str = "java";
if (str == null) {
    System.out.println("Null");
} else {
    System.out.println(str);
}
```

优化后代码：

```java
Optional<String> opt = Optional.of("java");
opt.ifPresentOrElse(v -> 
 System.out.println(v), () -> System.out.println("Null"));
```

> 小贴士：注意运行版本，必须是 JDK 9+ 才行。

## 6.使用多态

继承、封装和多态是 OOP（面向对象编程）的重要思想，这里使用多态的思想，提供一种去除 if else 方法。

优化前代码：

```java
Integer typeId = 0;
String type = "Name";
if ("Name".equals(type)) {
    typeId = 1;
} else if ("Age".equals(type)) {
    typeId = 2;
} else if ("Address".equals(type)) {
    typeId = 3;
}
```

使用多态，我们先定义一个接口，在接口中声明一个公共返回 `typeId` 的方法，在添加三个子类分别实现这三个子类，实现代码如下：

```java
public interface IType {
    public Integer getType();
}

public class Name implements IType {
    @Override
    public Integer getType() {
        return 1;
    }
}

public class Age implements IType {
    @Override
    public Integer getType() {
        return 2;
    }
}

public class Address implements IType {
    @Override
    public Integer getType() {
        return 3;
    }
}
```

> 注意：为了简便我们这里把类和接口放到了一个代码块中，在实际开发中应该分别创建一个接口和三个类分别存储。

此时，我们之前的 if else 判断就可以改为如下代码：

```java
IType itype = (IType) Class.forName("com.example." + type).newInstance();
Integer typeId = itype.getType();
```

此处只是提供一种实现思路和提供了一些简易版的代码，以供开发者在实际开发中，多一种思路和选择。

## 7.选择性的使用 switch

很多人都搞不懂 switch 和 if else 的使用场景，但在两者都能使用的情况下，可以尽量使用 switch，因为 switch 在常量分支选择时，switch 性能会比 if else 高。

if else 判断代码：

```java
if (cmd.equals("add")) {
    result = n1 + n2;
} else if (cmd.equals("subtract")) {
    result = n1 - n2;
} else if (cmd.equals("multiply")) {
    result = n1 * n2;
} else if (cmd.equals("divide")) {
    result = n1 / n2;
} else if (cmd.equals("modulo")) {
    result = n1 % n2;
}
```

switch 代码：

```java
switch (cmd) {
    case "add":
        result = n1 + n2;
        break;
    case "subtract":
        result = n1 - n2;
        break;
    case "multiply":
        result = n1 * n2;
        break;
    case "divide":
        result = n1 / n2;
        break;
    case "modulo":
        result = n1 % n2;
        break;
}
```

在 Java 14 可使用 switch 代码块，实现代码如下：

```java
// java 14
switch (cmd) {
    case "add" -> {
        result = n1 + n2;
    }
    case "subtract" -> {
        result = n1 - n2;
    }
    case "multiply" -> {
        result = n1 * n2;
    }
    case "divide" -> {
        result = n1 / n2;
    }
    case "modulo" -> {
        result = n1 % n2;
    }
}
```