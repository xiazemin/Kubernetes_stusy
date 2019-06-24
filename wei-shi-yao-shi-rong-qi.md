_传统_部署应用程序的方式，一般是使用操作系统自带的包管理器在主机上安装应用依赖，之后再安装应用程序。这无疑将应用程序的可执行文件、应用的配置、应用依赖库和应用的生命周期与宿主机操作系统进行了紧耦合。在此情境下，可以通过构建不可改变的虚拟机镜像版本，通过镜像版本实现可预测的发布和回滚，但是虚拟机实在是太重量级了，且镜像体积太庞大，便捷性差。

_新方式_是基于操作系统级虚拟化而不是硬件级虚拟化方法来部署容器。容器之间彼此隔离并与主机隔离：它们具有自己的文件系统，不能看到彼此的进程，并且它们所使用的计算资源是可以被限制的。它们比虚拟机更容易构建，并且因为它们与底层基础架构和主机文件系统隔离，所以它们可以跨云和操作系统快速分发。

由于容器体积小且启动快，因此可以在每个容器镜像中打包一个应用程序。这种一对一的应用镜像关系拥有很多好处。使用容器，不需要与外部的基础架构环境绑定, 因为每一个应用程序都不需要外部依赖，更不需要与外部的基础架构环境依赖。完美解决了从开发到生产环境的一致性问题。

容器同样比虚拟机更加透明，这有助于监测和管理。尤其是容器进程的生命周期由基础设施管理，而不是由容器内的进程对外隐藏时更是如此。最后，每个应用程序用容器封装，管理容器部署就等同于管理应用程序部署。

容器优点摘要:

* **敏捷的应用程序创建和部署**
  : 与虚拟机镜像相比，容器镜像更容易创建，提升了硬件的使用效率。
* **持续开发、集成和部署**
  : 提供可靠与频繁的容器镜像构建和部署，可以很方便及快速的回滚 \(由于镜像不可变性\).
* **关注开发与运维的分离**
  : 在构建/发布时创建应用程序容器镜像，从而将应用程序与基础架构分离。
* **开发、测试和生产环境的一致性**
  : 在笔记本电脑上运行与云中一样。
* **云和操作系统的可移植性**
  : 可运行在 Ubuntu, RHEL, CoreOS, 内部部署, Google 容器引擎和其他任何地方。
* **以应用为中心的管理**
  : 提升了操作系统的抽象级别，以便在使用逻辑资源的操作系统上运行应用程序。
* **松耦合、分布式、弹性伸缩**
  [**微服务**](http://martinfowler.com/articles/microservices.html)
  : 应用程序被分成更小，更独立的部分，可以动态部署和管理 - 而不是巨型单体应用运行在专用的大型机上。
* **资源隔离**
  : 通过对应用进行资源隔离，可以很容易的预测应用程序性能。
* **资源利用**
  : 高效率和高密度。

#### 为什么我们需要 Kubernetes，它能做什么? {#为什么我们需要-kubernetes-它能做什么}

最基础的，Kubernetes 可以在物理或虚拟机集群上调度和运行应用程序容器。然而，Kubernetes 还允许开发人员从物理和虚拟机’脱离’，从以**主机为中心**的基础架构转移到以**容器为中心**的基础架构，这样可以提供容器固有的全部优点和益处。Kubernetes 提供了基础设施来构建一个真正以**容器为中心**的开发环境。

Kubernetes 满足了生产中运行应用程序的许多常见的需求，例如：

* [Pod](https://kubernetes.io/docs/user-guide/pods/)
  提供复合应用并保留一个应用一个容器的容器模型,
* [挂载外部存储](https://kubernetes.io/docs/user-guide/volumes/)
  ,
* [Secret管理](https://kubernetes.io/docs/user-guide/secrets/)
  ,
* [应用健康检查](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)
  ,
* [副本应用实例](https://kubernetes.io/docs/user-guide/replication-controller/)
  ,
* [横向自动扩缩容](https://kubernetes.io/docs/user-guide/horizontal-pod-autoscaling/)
  ,
* [服务发现](https://kubernetes.io/docs/user-guide/connecting-applications/)
  ,
* [负载均衡](https://kubernetes.io/docs/user-guide/services/)
  ,
* [滚动更新](https://kubernetes.io/docs/user-guide/update-demo/)
  ,
* [资源监测](https://kubernetes.io/docs/user-guide/monitoring/)
  ,
* [日志采集和存储](https://kubernetes.io/docs/user-guide/logging/overview/)
  ,
* [支持自检和调试](https://kubernetes.io/docs/user-guide/introspection-and-debugging/)
  ,
* [认证和鉴权](https://kubernetes.io/docs/admin/authorization/)
  .

这提供了平台即服务 \(PAAS\) 的简单性以及基础架构即服务 \(IAAS\) 的灵活性，并促进跨基础设施供应商的可移植性。

有关详细信息，请参阅[用户指南](https://kubernetes.io/docs/user-guide/).

  


