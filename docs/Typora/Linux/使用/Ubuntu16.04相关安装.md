[TOC]

# Ubuntu16.04相关安装

## 安装open-vm-tools

```shell
sudo apt-get upgrate
sudo apt-get install open-vm-tools-desktop -y
sudo reboot
```

## 国内kernel源代码下载

网址 `http://ftp.sjtu.edu.cn/sites/ftp.kernel.org/pub/linux/kernel/`

## 内核编译相关

参考此篇博客：https://blog.csdn.net/qq_41175905/article/details/80529245

## 安装chrome浏览器

1. 将下载源加入到系统的源列表

   ```shell
   sudo wget http://www.linuxidc.com/files/repo/google-chrome.list -P /etc/apt/sources.list.d/
   ```

2. 导入谷歌软件的公钥，用于下面步骤中对下载软件进行验证。

   如果顺利的话，命令将返回OK

   ```shell
   wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -
   ```

3. 对当前系统的可用更新列表进行更新(不能省略)

   ```shell
   sudo apt-get update
   ```

4. 执行对谷歌 Chrome 浏览器（稳定版）的安装

   ```shell
   sudo apt-get install google-chrome-stable
   ```

5. 启动浏览器

   ```shell
   /usr/bin/google-chrome-stable
   ```

## 系统优化工具Stacer安装

1. `sudo add-apt-repository ppa:oguzhaninan/stacer -y`
2. `sudo apt-get update`
3. `sudo apt-get install stacer -y`

## deb文件的安装

```shell
sudo dpkg -i <package.deb>
//有需要依赖的类库没有安装，可先执行如下命令更新依赖包 再执行上面的命令
sudo apt-get -f install
```

## 卸载软件

```shell
apt-get --purge remove <package>				# 删除软件及其配置文件
apt-get autoremove <package>					# 删除没用的依赖包
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P		# 清理dpkg的列表中有“rc”状态的软件包
```

## 参考资料

1. ubuntu16.04安装google浏览器 https://www.cnblogs.com/remember-forget/p/10053630.html
2. 一篇很全面的博客（部分可用） Ubuntu16.04系统工作环境设置 https://blog.flyfox.top/2017/03/11/Ubuntu16-04系统工作环境设置/