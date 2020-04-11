# Linux shell

**Linux内核标准存放位置**

```shell
/boot
内核模块位置  /lib/modules/a.b.c
```



**通过dd命令生成指定大小文件**

```shell
dd if=/dev/zero of=test.100MB bs=1M count=100
```

**查看文件大小**

```shell
du -h filepath
```

```shell
ls -lh filepath
```

**使用objdump反汇编**

```shell
objdump -S read_write.o > test
```

