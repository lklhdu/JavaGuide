# 搭建Nginx流服务器

## 1.安装依赖

## 2.编译源码

新建一个nginx文件夹，用来存放需要下载的nginx和nginx-rtmp-module两个安装包源码

nginx[下载链接](http://nginx.org/en/download.html)，这里下载了1.8.1版本的源码，解压文件，生成nginx-1.8.1文件夹

在nginx目录下，下载nginx-rtmp-module

```shell
git clone https://github.com/arut/nginx-rtmp-module.git
```

然后编译安装nginx，cd进nginx的目录

```shell
cd nginx-1.8.1
./configure --add-module=../nginx-rtmp-module
make
make install
```

经过上述默认配置安装后，nginx目录如下

>* nginx安装目录 **/usr/local/nginx**
>* nginx配置目录 **/usr/local/nginx/conf/nginx.conf**
>* nginx运行目录 **/usr/local/nginx/sbin/nginx --options**

## 3.测试nginx

进入安装目录/usr/local/nginx，运行以下命令

```
./sbin/nginx
```

在浏览器中输入localhost:8080（或者服务器的IP），看到如下画面，表示安装成功

![20190624094425634](http://java-guide.oss-cn-hangzhou.aliyuncs.com/typora/20200715163202-638486.png)

## 4.配置rtmp

编辑/usr/local/nginx/conf/nginx.conf文件

以上步骤及细节可以参考博客[opencv读取rtsp图像处理后推流rtmp](https://blog.csdn.net/zong596568821xp/article/details/92790502)

## ubuntu下nginx停止、启动、重启

### 启动

启动代码格式：nginx运行目录地址 -c nginx配置文件地址

```shell
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
```

### 停止

1.查看进程号

```shell
ps -ef|grep nginx
```

2.杀死进程

```shell
kill -QUIT [master-pid]
```

### 重启

进入nginx可执行目录sbin下，输入命令 `./nginx -s reload`

### 参考资料

1. [ubuntu下nginx停止、启动、重启](https://blog.csdn.net/oMoDao1/article/details/83794458)