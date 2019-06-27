将资源暴露为新的Kubernetes Service。



指定deployment、service、replica set、replication controller或pod ，并使用该资源的选择器作为指定端口上新服务的选择器。deployment 或 replica set只有当其选择器可转换为service支持的选择器时，即当选择器仅包含matchLabels组件时才会作为暴露新的Service。



资源包括\(不区分大小写\)：



pod（po），service（svc），replication controller（rc），deployment（deploy），replica set（rs）



语法

$ expose \(-f FILENAME \| TYPE NAME\) \[--port=port\] \[--protocol=TCP\|UDP\] \[--target-port=number-or-name\] \[--name=name\] \[--external-ip=external-ip-of-service\] \[--type=type\]

示例

为RC的nginx创建service，并通过Service的80端口转发至容器的8000端口上。



kubectl expose rc nginx --port=80 --target-port=8000

由“nginx-controller.yaml”中指定的type和name标识的RC创建Service，并通过Service的80端口转发至容器的8000端口上。



kubectl expose -f nginx-controller.yaml --port=80 --target-port=8000



