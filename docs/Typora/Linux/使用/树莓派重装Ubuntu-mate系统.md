# 树莓派安装Ubuntu-mate系统

## 树莓派3安装ubuntu-mate系统

1. 格式化SD卡

   使用工具：SD Card Formatter

   下载地址：https://www.sdcard.org/downloads/formatter_4/eula_windows/

   select card -> Format

1. 烧录系统

   使用工具：win32diskimager

   下载地址：https://sourceforge.net/projects/win32diskimager/

   将下载的系统文件解压得到.img文件，需将该文件放在**英文目录下**

   左侧选择Image File,右侧选择SD卡设备，点击Write写入

1. 开始安装系统

   需要设备：HDMI连接显示器，键盘，鼠标

   通电后会出现ubuntu安装界面，按步骤安装即可（需联网）

## 树莓派4安装ubuntu-mate系统

大致的步骤和树莓派3的安装相同，不过使用的镜像需要更换成树莓派4对应的镜像。可以参考博客[树莓派4B 的折腾之旅【2020年4月27日更新】](https://blog.csdn.net/wwwmewww/article/details/104571436?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.nonecase)

## 树莓派4安装桌面

```shell
sudo apt install ubuntu-gnome-desktop
```

具体可以参考[树莓派安装ubuntu桌面](https://blog.csdn.net/hoojou/article/details/104494638)

## 更换国内下载源

### 更换步骤
1. 以root身份打开 /etc/apt/sources.list
1. 将 http://ports.ubuntu.com/ 全部替换为 http://mirrors.ustc.edu.cn/ubuntu-ports/ ，这是中科大的
1. 执行 `sudo apt-get update` 和 `sudo apt-get upgrade` 测试

### 查看是否提供arm支持的方法

打开一个镜像站点，然后以此打开 /dists/xenial/main/ ，看这个目录下面有没有 binary-arm 这样的字眼，如果有，就是提供arm支持的

### 参考资料

1. [【树莓派3】安装Ubuntu Mate系统 ](https://blog.csdn.net/henryheheng/article/details/78907406)
1.  [树莓派3B 安装ubuntu16.04 mate 详细教程](https://blog.csdn.net/Teddy_123/article/details/94330058)
1.  [【树莓派】为Ubuntu Mate for ARM更换中国软件源 ](https://blog.csdn.net/wr132/article/details/56700479)