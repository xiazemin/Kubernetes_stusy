Kubernetes 暴露服务的方式目前只有三种：LoadBlancer Service、NodePort Service、Ingress

Ingress 就是能利用 Nginx、Haproxy 啥的负载均衡器暴露集群内服务的工具；那么问题来了，集群内服务想要暴露出去面临着几个问题：

1.2、Pod 漂移问题

众所周知 Kubernetes 具有强大的副本控制能力，能保证在任意副本\(Pod\)挂掉时自动从其他机器启动一个新的，还可以动态扩容等，总之一句话，这个 Pod 可能在任何时刻出现在任何节点上，也可能在任何时刻死在任何节点上；那么自然随着 Pod 的创建和销毁，Pod IP 肯定会动态变化；那么如何把这个动态的 Pod IP 暴露出去？这里借助于 Kubernetes 的 Service 机制，Service 可以以标签的形式选定一组带有指定标签的 Pod，并监控和自动负载他们的 Pod IP，那么我们向外暴露只暴露 Service IP 就行了；这就是 NodePort 模式：即在每个节点上开起一个端口，然后转发到内部 Service IP 上

.3、端口管理问题

采用 NodePort 方式暴露服务面临一个坑爹的问题是，服务一旦多起来，NodePort 在每个节点上开启的端口会及其庞大，而且难以维护；这时候引出的思考问题是 “能不能使用 Nginx 啥的只监听一个端口，比如 80，然后按照域名向后转发？” 这思路很好，简单的实现就是使用 DaemonSet 在每个 node 上监听 80，然后写好规则，因为 Nginx 外面绑定了宿主机 80 端口\(就像 NodePort\)，本身又在集群内，那么向后直接转发到相应 Service IP 就行了

1.4、域名分配及动态更新问题

从上面的思路，采用 Nginx 似乎已经解决了问题，但是其实这里面有一个很大缺陷：每次有新服务加入怎么改 Nginx 配置？总不能手动改或者来个 Rolling Update 前端 Nginx Pod 吧？这时候 “伟大而又正直勇敢的” Ingress 登场，如果不算上面的 Nginx，Ingress 只有两大组件：Ingress Controller 和 Ingress

Ingress 这个玩意，简单的理解就是 你原来要改 Nginx 配置，然后配置各种域名对应哪个 Service，现在把这个动作抽象出来，变成一个 Ingress 对象，你可以用 yml 创建，每次不要去改 Nginx 了，直接改 yml 然后创建/更新就行了；那么问题来了：”Nginx 咋整？”

Ingress Controller 这东西就是解决 “Nginx 咋整” 的；Ingress Controoler 通过与 Kubernetes API 交互，动态的去感知集群中 Ingress 规则变化，然后读取他，按照他自己模板生成一段 Nginx 配置，再写到 Nginx Pod 里，最后 reload 一下

当然在实际应用中，最新版本 Kubernetes 已经将 Nginx 与 Ingress Controller 合并为一个组件，所以 Nginx 无需单独部署，只需要部署 Ingress Controller 即可

二、怼一个 Nginx Ingress

上面啰嗦了那么多，只是为了讲明白 Ingress 的各种理论概念，下面实际部署很简单

2.1、部署默认后端

我们知道 前端的 Nginx 最终要负载到后端 service 上，那么如果访问不存在的域名咋整？官方给出的建议是部署一个 默认后端，对于未知请求全部负载到这个默认后端上；这个后端啥也不干，就是返回 404，部署如下

➜  ~ kubectl create -f default-backend.yaml

deployment "default-http-backend" created

service "default-http-backend" created

这个 default-backend.yaml 文件可以在 官方 Ingress 仓库 找到

2.2、部署 Ingress Controller

部署完了后端就得把最重要的组件 Nginx+Ingres Controller\(官方统一称为 Ingress Controller\) 部署上

➜  ~ kubectl create -f nginx-ingress-controller.yaml

daemonset "nginx-ingress-lb" created

注意：官方的 Ingress Controller 有个坑，至少我看了 DaemonSet 方式部署的有这个问题：没有绑定到宿主机 80 端口，也就是说前端 Nginx 没有监听宿主机 80 端口\(这还玩个卵啊\)；所以需要把配置搞下来自己加一下 hostNetwork

当然它支持以 deamonset 的方式部署，这里用的就是\(个人喜欢而已\)，所以你发现我上面截图是 deployment，但是链接给的却是 daemonset，因为我截图截错了…..

2.3、部署 Ingress

这个可就厉害了，这个部署完就能装逼了

从上面可以知道 Ingress 就是个规则，指定哪个域名转发到哪个 Service，所以说首先我们得有个 Service，当然 Service 去哪找这里就不管了；这里默认为已经有了两个可用的 Service，以下以 Dashboard 和 kibana 为例

先写一个 Ingress 文件，语法格式啥的请参考 官方文档，由于我的 Dashboard 和 Kibana 都在 kube-system 这个命名空间，所以要指定 namespace，写之前 Service 分布如下

```
vim dashboard-kibana-ingress.yml

apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: dashboard-kibana-ingress
  namespace: kube-system
spec:
  rules:
  - host: dashboard.mritd.me
    http:
      paths:
      - backend:
          serviceName: kubernetes-dashboard
          servicePort: 80
  - host: kibana.mritd.me
    http:
      paths:
      - backend:
          serviceName: kibana-logging
          servicePort: 5601
```



