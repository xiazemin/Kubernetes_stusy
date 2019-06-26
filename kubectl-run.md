kubectl run

创建并运行一个或多个容器镜像。

创建一个deployment 或job 来管理容器。

语法：

$ run NAME --image=image \[--env="key=value"\] \[--port=port\] \[--replicas=replicas\] \[--dry-run=bool\] \[--overrides=inline-json\] \[--command\] -- \[COMMAND\] \[args...\]

示例：

启动nginx实例。



kubectl run nginx --image=nginx

启动hazelcast实例，暴露容器端口 5701。



kubectl run hazelcast --image=hazelcast --port=5701

启动hazelcast实例，在容器中设置环境变量“DNS\_DOMAIN = cluster”和“POD\_NAMESPACE = default”。



kubectl run hazelcast --image=hazelcast --env="DNS\_DOMAIN=cluster" --env="POD\_NAMESPACE=default"

启动nginx实例，设置副本数5。



kubectl run nginx --image=nginx --replicas=5

运行 Dry  打印相应的API对象而不创建它们。



kubectl run nginx --image=nginx --dry-run

