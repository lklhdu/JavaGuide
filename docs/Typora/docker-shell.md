```shell
/home/liyuegang/./login8260.sh //跳转到本机
/home/liyuegang/PI5000/./login.sh //跳转到虚拟机
cd /home/liyaogang/cloud_native_match/
vim exec.sh 
sh exec.sh 
docker ps //查看容器名
docker logs -ft [容器名]
docker stop $(docker ps -aq) //删除容器（当程序卡住时使用）
```

### jar包上传

```
#标签1
cd /home/liyuegang/
rz
scp tailbaseSampling-1.0-SNAPSHOT.jar root@10.1.100.213:/home/liyuegang/cloud_native_match/
#标签2
/home/liyuegang/./login8260.sh
cd /home/liyuegang/cloud_native_match
scp tailbaseSampling-1.0-SNAPSHOT.jar root@192.168.122.108:/home/liyaogang/cloud_native_match/
#标签3
docker rmi [镜像名]：[版本号]
docker build -t [镜像名]：[版本号] .
```



