Kubernetes集群包含有节点代理kubelet和Master组件\(APIs, scheduler, etc\)，一切都基于分布式的存储系统。下面这张图是Kubernetes的架构图。

![](https://img.kubernetes.org.cn/2016/10/20161028141542.jpg "20161028141542")

[**查看高清无码图**](https://raw.githubusercontent.com/kubernetes/kubernetes/release-1.2/docs/design/architecture.png)



## Kubernetes节点

在这张系统架构图中，我们把服务分为运行在工作节点上的服务和组成集群级别控制板的服务。

Kubernetes节点有运行应用容器必备的服务，而这些都是受Master的控制。

每次个节点上当然都要运行Docker。Docker来负责所有具体的映像下载和容器运行。

Kubernetes主要由以下几个核心组件组成：

* etcd保存了整个集群的状态；
* apiserver提供了资源操作的唯一入口，并提供认证、授权、访问控制、API注册和发现等机制；
* controller manager负责维护集群的状态，比如故障检测、自动扩展、滚动更新等；
* scheduler负责资源的调度，按照预定的调度策略将Pod调度到相应的机器上；
* kubelet负责维护容器的生命周期，同时也负责Volume（CVI）和网络（CNI）的管理；
* Container runtime负责镜像管理以及Pod和容器的真正运行（CRI）；
* kube-proxy负责为Service提供cluster内部的服务发现和负载均衡；

除了核心组件，还有一些推荐的Add-ons：

* kube-dns负责为整个集群提供DNS服务
* Ingress Controller为服务提供外网入口
* Heapster提供资源监控
* Dashboard提供GUI
* Federation提供跨可用区的集群
* Fluentd-elasticsearch提供集群日志采集、存储与查询

![](https://feisky.gitbooks.io/kubernetes/architecture/images/14791969222306.png)

![](https://feisky.gitbooks.io/kubernetes/architecture/images/14791969311297.png)

### 分层架构 {#分层架构}

Kubernetes设计理念和功能其实就是一个类似Linux的分层架构，如下图所示

![](https://feisky.gitbooks.io/kubernetes/architecture/images/14937095836427.jpg)

* 核心层：Kubernetes最核心的功能，对外提供API构建高层的应用，对内提供插件式应用执行环境
* 应用层：部署（无状态应用、有状态应用、批处理任务、集群应用等）和路由（服务发现、DNS解析等）
* 管理层：系统度量（如基础设施、容器和网络的度量），自动化（如自动扩展、动态Provision等）以及策略管理（RBAC、Quota、PSP、NetworkPolicy等）
* 接口层：kubectl命令行工具、客户端SDK以及集群联邦
* 生态系统：在接口层之上的庞大容器集群管理调度的生态系统，可以划分为两个范畴
  * Kubernetes外部：日志、监控、配置管理、CI、CD、Workflow、FaaS、OTS应用、ChatOps等
  * Kubernetes内部：CRI、CNI、CVI、镜像仓库、Cloud Provider、集群自身的配置和管理等

## kubelet

kubelet负责管理[pods](https://www.kubernetes.org.cn/kubernetes-pod)和它们上面的容器，images镜像、volumes、etc。

## kube-proxy

每一个节点也运行一个简单的网络代理和负载均衡（详见[services FAQ](https://github.com/kubernetes/kubernetes/wiki/Services-FAQ) \)（PS:官方 英文）。 正如Kubernetes API里面定义的这些服务（详见[the services doc](https://github.com/kubernetes/kubernetes/blob/release-1.2/docs/user-guide/services.md)）（PS:官方 英文）也可以在各种终端中以轮询的方式做一些简单的TCP和UDP传输。

服务端点目前是通过DNS或者环境变量\( Docker-links-compatible 和 Kubernetes{FOO}\_SERVICE\_HOST 及 {FOO}\_SERVICE\_PORT 变量都支持\)。这些变量由服务代理所管理的端口来解析。

## Kubernetes控制面板

Kubernetes控制面板可以分为多个部分。目前它们都运行在一个_master_ 节点，然而为了达到高可用性，这需要改变。不同部分一起协作提供一个统一的关于集群的视图。

## etcd

所有master的持续状态都存在etcd的一个实例中。这可以很好地存储配置数据。因为有watch\(观察者\)的支持，各部件协调中的改变可以很快被察觉。

## Kubernetes API Server

API服务提供[Kubernetes API](https://github.com/kubernetes/kubernetes/blob/release-1.2/docs/api.md) （PS:官方 英文）的服务。这个服务试图通过把所有或者大部分的业务逻辑放到不两只的部件中从而使其具有CRUD特性。它主要处理REST操作，在etcd中验证更新这些对象（并最终存储）。

## Scheduler

调度器把未调度的pod通过binding api绑定到节点上。调度器是可插拔的，并且我们期待支持多集群的调度，未来甚至希望可以支持用户自定义的调度器。

## Kubernetes控制管理服务器

所有其它的集群级别的功能目前都是由控制管理器所负责。例如，端点对象是被端点控制器来创建和更新。这些最终可以被分隔成不同的部件来让它们独自的可插拔。

[replicationcont](https://github.com/kubernetes/kubernetes/blob/release-1.2/docs/user-guide/replication-controller.md)[roller](https://github.com/kubernetes/kubernetes/blob/release-1.2/docs/user-guide/replication-controller.md)（PS:官方 英文）是一种建立于简单的 [pod](https://www.kubernetes.org.cn/kubernetes-pod)API之上的一种机制。一旦实现，我们最终计划把这变成一种通用的插件机制。

参考：

https://github.com/kubernetes/kubernetes/blob/release-1.2/docs/design/architecture.md

https://feisky.gitbooks.io/kubernetes/architecture/architecture.html

