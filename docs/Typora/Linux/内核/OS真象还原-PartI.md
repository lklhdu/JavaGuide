# OS真象还原-PartI

**编译print.S**

```shell
nasm -f elf -o lib/kernel/print.o lib/kernel/print.S
```

**编译 main.c**

```shell
gcc -I lib/kernel/ -c -o kernel/main.o kernel/main.c
```

**链接 main.o 和 print.o** 

```shell
ld -Ttext 0xc0001500 -e main -o kernel.bin \ 
> kernel/main.o lib/kernel/print.o
```

注意，上面 ld 命令第一行后的斜杠'\'不属于命令本身，用于一行写不下全部命令参数的情况。 

**写入虚拟硬盘** 

```shell
dd if=kernel.bin of=/home/lkl/bochs/hd60M.img bs=512 count=200 seek=9 conv=notrun
```

### makefile

**chapter8**开始，可以将所有的命令写入到`makefile`文件中，然后执行命令`make all`

### 需要修改的部分

* gcc编译需要添加参数`-fno-stack-protector`
* `dd`需要将路径更换为本机的工作目录