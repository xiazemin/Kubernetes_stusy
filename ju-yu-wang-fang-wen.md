私有网段,有A,B,C三个地址段：

10.0.0.0/8:10.0.0.0-10.255.255.255

172.16.0.0/12:172.16.0.0-172.31.255.255

192.168.0.0/16:192.168.0.0-192.168.255.255

假设我们有 A , B , C 3台机器



A: 192.168.1.10



B: 192.168.1.11



C: 192.168.1.12



现在A上输入



docker network create -d macvlan --subnet=192.168.1.0/24 --gateway=192.168.1.1 -o parent=eth0 mynet

创建一个macvlan的网络，网络名为mynet  继承网卡eth0的属性



分别在B和C上输入相同的命令



这样我们就创建了3个同样网络，分别在3个不同的机器上



使用命令创建docker



docker run --restart=always --net=mynet --name="test1" --ip=192.168.1.100 -v /jastme/test1:/testl --privileged=true --cpu-shares 1024 -m 4096 -dit a9ff415eb22b /bin/bash



docker run --restart=always --net=mynet --name="test1" --ip=192.168.1.101 -v /jastme/test1:/testl --privileged=true --cpu-shares 1024 -m 4096 -dit a9ff415eb22b /bin/bash



docker run --restart=always --net=mynet --name="test1" --ip=192.168.1.102 -v /jastme/test1:/testl --privileged=true --cpu-shares 1024 -m 4096 -dit a9ff415eb22b /bin/bash

分别在3台机器上创建3个docker容器



然在进入容器ssh到其他容器



你可以发现网络都是通的，这样，局域网就成功创建好了。

