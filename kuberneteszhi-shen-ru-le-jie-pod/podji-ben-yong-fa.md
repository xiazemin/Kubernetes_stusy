```
在使用docker时，我们可以使用docker run命令创建并启动一个容器，而在Kubernetes系统中对长时间运行的容器要求是：其主程序需要一直在前台运行。如果我们创建的docker镜像的启动命令是后台执行程序，例如Linux脚本：
　　nohup ./startup.sh &
　　则kubelet创建包含这个容器的pod后运行完该命令，即认为Pod执行结束，之后根据RC中定义的pod的replicas副本数量生产一个新的pod，而一旦创建出新的pod，将在执行完命令后陷入无限循环的过程中，这就是Kubernetes需要我们创建的docker镜像以一个前台命令作为启动命令的原因。
　　对于无法改造为前台执行的应用，也可以使用开源工具supervisor辅助进行前台运行的功能。
****Pod可以由一个或多个容器组合而成
例如：两个容器应用的前端frontend和redis为紧耦合的关系，应该组合成一个整体对外提供服务，则应该将这两个打包为一个pod.
配置文件frontend-localredis-pod.yaml如下：

apiVersion:v1
kind: Pod
metadata:
  name: redis-php
  label:
    name: redis-php
spec:
  containers:
  - name: frontend
    image: kubeguide/guestbook-php-frontend:localredis
    ports:
    - containersPort: 80
  - name: redis-php
    image:kubeguide/redis-master
    ports:
    - containersPort: 6379
　　

　　属于一个Pod的多个容器应用之间相互访问只需要通过localhost就可以通信，这一组容器被绑定在一个环境中。
　　使用kubectl create创建该Pod后，get Pod信息可以看到如下图：

#kubectl get gods
NAME READY STATUS RESTATS AGE
redis-php 2/2Running 0 10m
　　可以看到READY信息为2/2，表示Pod中的两个容器都成功运行了.

　　查看pod的详细信息，可以看到两个容器的定义和创建过程。


[root@kubernetes-master ~]# kubectl describe redis-php
the server doesn't have a resourcetype "redis-php"
[root@kubernetes-master ~]# kubectl describe pod redis-php
Name: redis-php
Namespace: default
Node: kubernetes-minion/10.0.0.23
Start Time: Wed, 12 Apr 2017 09:14:58 +0800
Labels: name=redis-php
Status: Running
IP: 10.1.24.2
Controllers: <none>
Containers:
nginx:
Container ID: docker://d05b743c200dff7cf3b60b7373a45666be2ebb48b7b8b31ce0ece9be4546ce77
Image: nginx
Image ID: docker-pullable://docker.io/nginx@sha256:e6693c20186f837fc393390135d8a598a96a833917917789d63766cab6c59582
Port: 80/TCP
State: Running
Started: Wed, 12 Apr 2017 09:19:31 +0800
　　
```



