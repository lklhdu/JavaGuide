# idea打包springboot项目

1. File -> project Structure -> Artifacts -> JAR -> From modules with dependencies

1. 选择main class 

1. 选择copy to the output，**修改MANIFEST.MF 文件存放的目录，建议放在resources下**（可以避免报Error: Invalid or corrupt jarfile x.jar 的错误）

1. build 打包

   Build Artifacts -> Build -> ...

1. 把jar包放到服务器上运行

### 参考资料

1. [idea打包 springboot项目所遇到的坑](https://www.geek-share.com/detail/2767262380.html)