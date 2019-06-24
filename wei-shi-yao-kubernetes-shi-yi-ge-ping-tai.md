Kubernetes 提供了很多的功能，总会有新的场景受益于新特性。它可以简化应用程序的工作流，加快开发速度。被大家认可的应用编排通常需要有较强的自动化能力。这就是为什么 Kubernetes 被设计作为构建组件和工具的生态系统平台，以便更轻松地部署、扩展和管理应用程序。

[Label](https://kubernetes.io/docs/user-guide/labels/)允许用户按照自己的方式组织管理对应的资源。[注解](https://kubernetes.io/docs/user-guide/annotations/)使用户能够以自定义的描述信息来修饰资源，以适用于自己的工作流，并为管理工具提供检查点状态的简单方法。

此外，[Kubernetes 控制面 \(Control Plane\)](https://kubernetes.io/docs/admin/cluster-components)是构建在相同的[APIs](https://kubernetes.io/docs/api/)上面，开发人员和用户都可以用。用户可以编写自己的控制器，[调度器](https://github.com/kubernetes/kubernetes/tree/master/docs/devel/scheduler.md)等等，如果这么做，根据新加的[自定义 API](https://github.com/kubernetes/kubernetes/blob/master/docs/design/extending-api.md)，可以扩展当前的通用[CLI 命令行工具](https://kubernetes.io/docs/user-guide/kubectl-overview/)。

这种[设计](https://git.k8s.io/community/contributors/design-proposals/architecture/principles.md)使得许多其他系统可以构建在 Kubernetes 之上。

#### Kubernetes 不是什么: {#kubernetes-不是什么}

Kubernetes 不是一个传统意义上，包罗万象的 PaaS \(平台即服务\) 系统。我们保留用户选择的自由，这非常重要。

* Kubernetes 不限制支持的应用程序类型。 它不插手应用程序框架 \(例如
  [Wildfly](http://wildfly.org/)
  \), 不限制支持的语言运行时 \(例如 Java, Python, Ruby\)，只迎合符合
  [12种因素的应用程序](http://12factor.net/)
  ，也不区分”应用程序”与”服务”。Kubernetes 旨在支持极其多样化的工作负载，包括无状态、有状态和数据处理工作负载。如果应用可以在容器中运行，它就可以在 Kubernetes 上运行。
* Kubernetes 不提供作为内置服务的中间件 \(例如 消息中间件\)、数据处理框架 \(例如 Spark\)、数据库 \(例如 mysql\)或集群存储系统 \(例如 Ceph\)。这些应用可以运行在 Kubernetes 上。
* Kubernetes 没有提供点击即部署的服务市场
* Kubernetes 从源代码到镜像都是非垄断的。 它不部署源代码且不构建您的应用程序。 持续集成 \(CI\) 工作流是一个不同用户和项目都有自己需求和偏好的领域。 所以我们支持在 Kubernetes 分层的 CI 工作流，但不指定它应该如何工作。
* Kubernetes 允许用户选择其他的日志记录，监控和告警系统 \(虽然我们提供一些集成作为概念验证\)
* Kubernetes 不提供或授权一个全面的应用程序配置语言/系统 \(例如
  [jsonnet](https://github.com/google/jsonnet)
  \).
* Kubernetes 不提供也不采用任何全面机器配置、保养、管理或自我修复系统

另一方面，许多 PaaS 系统_运行_在 Kubernetes 上面，例如[Openshift](https://github.com/openshift/origin),[Deis](http://deis.io/), and[Eldarion](http://eldarion.cloud/)。 您也可以自定义您自己的 PaaS, 与您选择的 CI 系统集成，或与 Kubernetes 一起使用: 将您的容器镜像部署到 Kubernetes。

由于 Kubernetes 在应用级别而不仅仅在硬件级别上运行，因此它提供 PaaS 产品通用的一些功能，例如部署、扩展、负载均衡、日志记录、监控等。但是，Kubernetes 不是单一的，默认解决方案是可选和可插拔的。

此处，Kubernetes 不仅仅是一个 “编排系统”；它消除了编排的需要。 “编排”技术定义的是工作流的执行: 从 A 到 B，然后到 C。相反，Kubernetes 是包括一套独立、可组合的控制过程，通过声明式语法使其连续地朝着期望状态驱动当前状态。 不需要告诉它具体从 A 到 C 的过程，只要告诉到 C 的状态即可。 也不需要集中控制；该方法更类似于”编舞”。这使得系统更容易使用并且更强大、更可靠、更具弹性和可扩展性。

