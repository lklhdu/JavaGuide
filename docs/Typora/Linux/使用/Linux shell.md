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

**解决vs code修改文件时提示没有权限的问题**

```shell
sudo chown -R lkl /home/lkl/mit/lab
```

**跨机远程拷贝scp**

本地服务器 -> 远程服务器

```shell
$scp [-r] local_file remote_username@remote_ip:remote_folder
$scp [-r] local_file remote_username@remote_ip:remote_file
```

加上`-r`表示复制目录文件，不加表示复制普通文件

远程服务器 -> 本地服务器

**使用示例**

```shell
$scp root@10.6.159.147:/opt/soft/demo.tar /opt/soft/
```

表示从`10.6.159.147`机器上的`/opt/soft/`的目录中下载`demo.tar `文件到本地`/opt/soft/`目录中

```shell
$scp -r root@10.6.159.147:/opt/soft/test /opt/soft/
```

表示从`10.6.159.147`机器上的`/opt/soft/`中下载`test`目录到本地的`/opt/soft/`目录来