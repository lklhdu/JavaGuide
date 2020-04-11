# Java NIO Path路径

一个Path实例代表一个文件系统内的路径。path可以指向文件也可以指向目录。

为了使用`java.nio.file.Path`实例我们必须创建`Path`对象。创建`Path`实例可以通过`Paths`的工厂方法`get()`。

```java 
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

public void pathUsage() {
  Path currentDir = Paths.get("."); // currentDir = "."
  Path fullPath = currentDir.toAbsolutePath(); // fullPath = "/Users/guest/workspace"
  Path one = currentDir.resolve("file.txt"); // one = "./file.txt"
  Path fileName = one.getFileName(); // fileName = "file.txt"
}
```



### 参考文献

1. Java NIO Path路径 https://wiki.jikexueyuan.com/project/java-nio-zh/java-nio-path.html
2. Best Java code snippets using [java.nio.file] Path.toAbsolutePath  https://www.codota.com/code/java/methods/java.nio.file.Path/toAbsolutePath