# GCC命令&gdb调试

## GCC命令

### 简单编译

```shell
gcc test.c -o test
```

### 实际过程

实质上，上述编译过程分为四个阶段进行的，即预编译->编译->汇编->链接

```shell
# 预处理
gcc -E test.c -o test.i
# 编译
gcc -S test.i -o test.s
# 汇编
gcc -c test.s -o test.o
# 链接
gcc test.o -o test
```

### 多个程序文件编译

```shell
gcc test1.c test2.c -o test
```

### 检错

建议加上编译选项-Wall，使用它能够使GCC产生尽可能多的警告信息。

```shell
gcc -Wall illcode.c -o illcode
```

### 参考资料

1. Linux GCC常用命令 https://www.cnblogs.com/ggjucheng/archive/2011/12/14/2287738.html

## gdb调试

### 1.启动gdb

编译前加上-g选项

```shell
gcc -g test.c -o test
```

调试可执行文件

```shell
gdb <program>
```

调试core文件

```shell
gdb <program> <core dump file>
```

### 2.gdb交互

| 命令                         | 解释                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| r                            | Run的简写，运行被调试的程序。 如果此前没有下过断点，则执行完整个程序；如果有断点，则程序暂停在第一个可用断点处。 |
| c                            | Continue的简写，继续执行被调试程序，直至下一个断点或程序结束。 |
| b<>,d                        | b: Breakpoint的简写，设置断点。两可以使用“行号”“函数名称”“执行地址”等方式指定断点位置。d:删除指定编号的某个断点，或删除所有断点。 |
| s, n                         | s: 执行一行源程序代码，如果此行代码中有函数调用，则进入该函数； n: 执行一行源程序代码，此行代码中的函数调用也一并执行。 |
| si, ni                       | si命令类似于s命令，ni命令类似于n命令。所不同的是，这两个命令（si/ni）所针对的是汇编指令，而s/n针对的是源代码。 |
| p <变量名称>                 | Print的简写，显示指定变量（临时变量或全局变量）的值。        |
| display ... undisplay <编号> | display，设置程序中断后欲显示的数据及其格式。  undispaly，取消先前的display设置，编号从1开始递增。 |
| q                            | Quit的简写，退出GDB调试环境。                                |
| help [命令名称]              | GDB帮助命令，提供对GDB名种命令的解释说明。                   |

### 3.常用指令

```shell
gdb <program>
# 设置断点
(gdb)b main
(gdb)b 20
# 执行被调试程序
(gdb)r
# 单步调试
# 小技巧：直接回车的作用是重复上一指令，对于单步调试非常方便
(gdb)s
# 打印变量的值
(gdb)p n
# 继续执行被调试程序
(gdb)c

# 其他
# 在每次程序中断时显示下一条汇编指令
(gdb)display/i $pc
# 针对汇编指令的单步调试
(gdb)si
# 删除所以断点
(gdb)d
# 在 main 函数的 prolog 代码处设置断点
(gdb)b *main
(gdb)r
(gdb)si
# 显示寄存器的当前值
(gdb)i r
# 退出gdb调试
(gdb)q
```



### 参考资料

1. gdb调试利器 https://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/gdb.html
2. gdb命令 https://man.linuxde.net/gdb