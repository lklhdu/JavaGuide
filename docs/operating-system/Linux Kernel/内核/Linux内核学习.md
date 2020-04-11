# Linux内核学习

1. 试着以比较粗的粒度对以下几个方面有大致的印象
   1. 系统调用接口
   2. `memory`
   3. `process and schedule`
   4. 块IO和文件系统
   5. 网络协议栈
   6. 驱动相关
2. 试着写一个文件系统，当`touch`一个大文件时，将这个文件挂到写的文件系统上，看实现的函数能否使用（`cd`、`mkdir`等）

### 相关学习网站

1. https://lwn.net/ 致力于从Linux和自由软件开发社区内部获得最佳报道
2. https://elixir.bootlin.com/ 源代码在线阅读
3. https://wiki.osdev.org/Main_Page 最大的操作系统开发人员在线社区，可以学习如何编写自己的操作系统