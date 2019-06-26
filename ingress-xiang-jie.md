Kubernetes 暴露服务的方式目前只有三种：LoadBlancer Service、NodePort Service、Ingress

Ingress 就是能利用 Nginx、Haproxy 啥的负载均衡器暴露集群内服务的工具；那么问题来了，集群内服务想要暴露出去面临着几个问题：



1.2、Pod 漂移问题

众所周知 Kubernetes 具有强大的副本控制能力，能保证在任意副本\(Pod\)挂掉时自动从其他机器启动一个新的，还可以动态扩容等，总之一句话，这个 Pod 可能在任何时刻出现在任何节点上，也可能在任何时刻死在任何节点上；那么自然随着 Pod 的创建和销毁，Pod IP 肯定会动态变化；那么如何把这个动态的 Pod IP 暴露出去？这里借助于 Kubernetes 的 Service 机制，Service 可以以标签的形式选定一组带有指定标签的 Pod，并监控和自动负载他们的 Pod IP，那么我们向外暴露只暴露 Service IP 就行了；这就是 NodePort 模式：即在每个节点上开起一个端口，然后转发到内部 Service IP 上

