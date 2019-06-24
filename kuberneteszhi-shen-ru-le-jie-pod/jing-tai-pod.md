```
静态pod是由kubelet进行管理的仅存在于特定Node的Pod上，他们不能通过API Server进行管理，无法与ReplicationController、Deployment或者DaemonSet进行关联，并且kubelet无法对他们进行健康检查。静态Pod总是由kubelet进行创建，并且总是在kubelet所在的Node上运行。
创建静态Pod有两种方式：配置文件或者HTTP方式
1）配置文件方式
　　首先，需要设置kubelet的启动参数"--config",指定kubelet需要监控的配置文件所在的目录，kubelet会定期扫描该目录，冰根据目录中的 .yaml或 .json文件进行创建操作
假设配置目录为/etc/kubelet.d/配置启动参数:--config=/etc/kubelet.d/,然后重启kubelet服务后，再宿主机受用docker ps或者在Kubernetes Master上都可以看到指定的容器在列表中
由于静态pod无法通过API Server直接管理，所以在master节点尝试删除该pod，会将其变为pending状态，也不会被删除

#kubetctl delete pod static-web-node1
pod "static-web-node1"deleted
#kubectl get pods
NAME READY STATUS RESTARTS AGE
static-web-node1 0/1Pending 0 1s
　　

　　要删除该pod的操作只能在其所在的Node上操作，将其定义的.yaml文件从/etc/kubelet.d/目录下删除
1
2
#rm -f /etc/kubelet.d/static-web.yaml
#docker ps
　　
```



