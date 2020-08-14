# java 方法中传递的参数

在java方法中改变参数的值是行不通的。但是可以改变引用变量的属性值。

总结后就是一下几句话：

1. 对于基本类型参数，在方法体内对参数进行重新赋值，并不会改变原有变量的值。
1. 对于引用类型参数，在方法体内对参数进行重新赋予引用，并不会改变原有变量所持有的引用。 
1. 方法体内对参数进行运算，不影响原有变量的值。 
1. **方法体内对参数所指向对象的属性进行操作，将改变原有变量所指向对象的属性值。(比如数组、集合内的元素)**

举例：

```java
public class Main {

    private static void getMiddleOne(boolean b, Boolean boo, Boolean[] arr){
        b = true;  //形参，不会改变原有值
        boo = new Boolean(true);  //引用变量的直接操作相当于值传递，不会改变原来的引用变量
        arr[0] = true;  //引用变量的属性的操作，会改变原有引用的属性，相当于传址调用
    }

    //测试
    public static void main(String[] args) {
        boolean b = false;
        Boolean boo = new Boolean(false);
        Boolean[] arr = new Boolean[]{false};

        getMiddleOne(b, boo, arr);

        System.out.println(b);  
        System.out.println(boo.toString());
        System.out.println(arr[0]);

        /**
		 * output:
		 * 		false
		 * 		false
		 *		true
		 */
    }
}
```

转载自[在java方法中改变传递的参数的值](https://blog.csdn.net/sinat_22013331/article/details/51150358)