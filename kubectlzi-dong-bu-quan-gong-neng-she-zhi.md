执行：

1、yum install -y bash-completion

2、source &lt;\(kubectl completion bash\)

3、echo "source &lt;\(kubectl completion bash\)" &gt;&gt; ~/.bashrc



1、创建

nginx-test：控制器的名称，默认是deployment控制器

--image：nginx:1.14 镜像

--port=80：暴露的端口

--replicas=3：启动3个副本



kubectl run nginx-test --image=nginx:1.14 --port=80 --replicas=3



2、查看

kubectl get pods

kubectl get pods,deployment,replicaset



kubectl api-resources 查看简写的命令

比如：service 就是cs

componentstatuses就是cs



3、若是有异常

kubectl describe pods nginx-test-795c895f4c-zpr87



4、删除

kubectl delete deploy/nginx-test



5、创建好控制器后，将 它发布出去

--port=80：集群内部之间访问的端口

-type=NodePort：指定这个类型，外部能访问

--target-port=80：容器的端口

--name=nginx-service：指定service名称



kubectl expose deployment nginx-test --port=80 --type=NodePort --target-port=80 --name=nginx-service





