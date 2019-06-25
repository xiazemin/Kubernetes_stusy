概论 : Kubernetes中概念的简要概述

* Cluster : 集群是指由Kubernetes使用一系列的物理机、虚拟机和其他基础资源来运行你的应用程序。
* Node : 一个node就是一个运行着Kubernetes的物理机或虚拟机，并且pod可以在其上面被调度。.
* [**Pod**:](https://www.kubernetes.org.cn/kubernetes-pod)
  一个pod对应一个由相关容器和卷组成的容器组 （
  [**了解Pod详情**](https://www.kubernetes.org.cn/kubernetes-pod)
  ）
* Label : 一个label是一个被附加到资源上的键/值对，譬如附加到一个Pod上，为它传递一个用户自定的并且可识别的属性.Label还可以被应用来组织和选择子网中的资源（了解Label详情）
* selector是一个通过匹配labels来定义资源之间关系得表达式，例如为一个负载均衡的service指定所目标Pod.（了解selector详情）
* [**Replication Controller**](https://www.kubernetes.org.cn/replication-controller-kubernetes)
  : replication controller 是为了保证一定数量被指定的Pod的复制品在任何时间都能正常工作.它不仅允许复制的系统易于扩展，还会处理当pod在机器在重启或发生故障的时候再次创建一个（
  [**了解Replication Controller详情**](https://www.kubernetes.org.cn/replication-controller-kubernetes)
  ）
* [**Service**](https://www.kubernetes.org.cn/kubernetes-services)
  : 一个service定义了访问pod的方式，就像单个固定的IP地址和与其相对应的DNS名之间的关系。（
  [**了解Service详情**](https://www.kubernetes.org.cn/kubernetes-services)
  ）
* [**Volume**](https://www.kubernetes.org.cn/kubernetes-volumes)
  : 一个volume是一个目录，可能会被容器作为未见系统的一部分来访问。（
  [**了解Volume详情**](https://www.kubernetes.org.cn/kubernetes-volumes)
  ）
* Kubernetes volume 构建在Docker Volumes之上,并且支持添加和配置volume目录或者其他存储设备。
* Secret : Secret 存储了敏感数据，例如能允许容器接收请求的权限令牌。
* Name : 用户为Kubernetes中资源定义的名字
* Namespace : Namespace 好比一个资源名字的前缀。它帮助不同的项目、团队或是客户可以共享cluster,例如防止相互独立的团队间出现命名冲突
* Annotation : 相对于label来说可以容纳更大的键值对，它对我们来说可能是不可读的数据，只是为了存储不可识别的辅助数据，尤其是一些被工具或系统扩展用来操作的数据

  


