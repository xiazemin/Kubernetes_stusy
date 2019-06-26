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

5、更新image的版本，现在nginx 版本为1.14,先需要更新到1.15版本

指定控制器名称

nginx-test：容器的名字，可根据kubectl describe deployment/nginx-test 查看



\[root@docker ~\]\# kubectl set image deployment/nginx-test nginx-test=nginx:1.15



6、再访问curl http://192.168.1.23:34234 可以看到nginx版本变为1.15



7、更新有问题需要回滚

kubectl rollout history deployment/nginx-test

kubectl rollout undo deployment/nginx-test 回滚到上一个版本



8、再次验证：http://192.168.1.23:34234 可以看到nginx版本变为1.14



9、删除

如果删除pod，控制器会帮你再新建一个pod，所以需要删除控制器

kubectl delete deploy/nginx-test

kubectl delete svc/nginx-service



《三》kubectl命令行管理工具、YAML配置详解



10、可以进入pod

kubectl exec -it pod名称 bash

