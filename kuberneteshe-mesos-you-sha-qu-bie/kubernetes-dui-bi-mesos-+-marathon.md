“Kubernetes是一个自动化，容器化应用程序部署扩展和管理的开放源代码系统。”Kubernetes由Google根据他们在生产中运行容器的经验使用称为Borg的内部集群管理系统（有时简称Omega）。 Kubernetes的体系结构依赖于这种经验，如下所示：











从上图可以看出，有一些与Kubernetes集群相关的组件。主节点将容器工作负载放置在工作节点。其他组件包括：etcd：该组件存储配置数据，可以通过简单的HTTP或JSON API由Kubernetes master API服务器访问\(可以看作集群数据库\)。APIserver：此组件是Kubernetes主节点的管理中心。它有助于各个组件之间的通信，从而保持集群的健康。Controller Manager：该组件确保通过向上和向下扩展工作负载来使群集的期望状态与当前状态相匹配。Scheduler：该组件将工作负载放置在适当的节点上。Kubelet：该组件从API服务器接收pod规范并管理在主机中运行的pod。以下列表提供了与Kubernetes相关的一些其他常用术语：pod：Kubernetes编排调度容器以pod 为单位。同一个pod中的容器运行在同一个节点上，并共享资源，如文件系统，内核命名空间和IP地址。Deployments：可以创建管理一组pod。部署可以与服务层一起用于水平扩展或确保可用性。Services：可以直接访问。 Kubernetes为集群提供一个DNS服务器，用于监视新服务，并允许他们通过名称进行寻址。服务是您的容器工作负载的“外在表现”。Labels：关联到实际各种对象的键值对。它们可以用来搜索和更新多个对象作为一个单一集合。（例如：可以用标签区分，实验环境，测试环境）Mesos + Marathon概述Mesos是一个分布式内核，旨在为您的数据中心提供动态的资源分配。想象一下，你管理一个中型企业的IT部门。您需要在一天中的100个节点上运行工作负载，但是需要在25个小时后运行。 Mesos可以重新分配工作负载，以便其他75个节点在不使用时可以关闭电源。 Mesos也可以提供资源共享。如果您的某个节点发生故障，则可以将工作负载分配到其他节点中。Mesos带有许多使用其资源共享功能的框架和应用栈。每个框架由一个调度器和一个执行器组成。 Marathon是一个框架（或元框架），可以启动应用程序和其他框架。 Marathon还可以作为容器编排平台，为容器工作负载提供扩展和自我修复。下图显示了Mesos + Marathon的架构。











Mesos和Marathon中有许多不同的组件。以下列表提供了一些常用术语：Mesos Master：master这种类型的节点可以给多种框架共享资源，例如用于容器编排的Marathon，用于大规模数据处理的Spark以及用于NoSQL数据库的Cassandra。Mesos Slave：Slave这种类型的节点执行实际工作任务，并且向master汇报可用资源。Framework：Framework向Master注册，master允许Framework的任务在slave节点执行。Zookeeper：这个组件提供了一个高度可用的数据库，这个数据库可以让群集保持现在的状态，并且稳定。Marathon Scheduler：这个组件接收来自Mesos master的报告。 Mesos master 提供当前集群可用的内存，CPUDocker Executor：这个组件接收来自Marathon调度器的任务，并启动slave节点上运行容器。Mesosphere DCOSMesosphere Enterprise DC / OS利用Mesos分布式系统内核，在容器和大数据管理基础上构建，提供安装，用户界面，管理和监视工具等功能。下图显示了DCOS的高级体系结构。











kubernetes 对比 mesos + marathon1. 应用定义k8s: 可以使用Pod，部署和服务的组合来部署应用程序。一个pod是一组位于同一节点的容器，是部署的原子单位。部署可以在多个节点上具有副本。服务是容器工作负载的“外部表现”，并与DNS集成配合访问。marathon:从用户的角度来看，应用程序将作为Marathon在节点上调度的任务运行。 对于Mesos，应用程序是一个框架，可以是Marathon，Cassandra，Spark等等。 Marathon将容器调度为在从节点上执行的任务。marathon 1.4引入了pod 的概念（ 如同 Kubernetes pod），但这不是marathon核心的一部分。 节点可以根据机架，连接的存储类型等进行标记。启动Docker容器时可以使用这些约束。2.应用的可扩展性k8s：每个应用程序层都被定义为一个pod，并且可以在通过声明性指定的部署进行管理时进行缩放，例如，在YAML中。 缩放可以是手动或自动的。marathon:可以使用Mesos CLI或UI。 可以使用JSON定义来启动Docker容器，这些定义指定了存储库，资源，实例数量和要执行的命令。 可以通过使用Marathon UI进行扩展，Marathon调度程序将根据指定的标准将这些容器分布在从节点上。 支持自动缩放。 可以使用应用程序组来部署多层应用程序。3.高可用k8s:pod可以部署在不同节点上支持高可用。多个master节点，node 节点可以以负载均衡的方式对应客户端的访问。etcd 可以以集群方式部署marathon: 容器可以不受限制的部署在任何节点上。使用Zookeeper支持Mesos和Marathon的高可用性。 Zookeeper提供Mesos和Marathon领导者的选举并维护集群状态。4.负载均衡k8s: Pod是通过服务暴露的，可以在集群内用作负载平衡器。marathon :主机端口可以映射到多个容器端口，作为其他应用程序或最终用户的前端。5.应用程序自动伸缩k8s:使用简单的pod目标进行自动缩放是使用部署以声明方式定义的。 还支持使用资源度量的自动缩放。 资源指标范围从CPU和内存利用率到请求或每秒数据包甚至自定义指标。marathon:马拉松持续监视正在运行的Docker容器实例的数量。 如果其中一个容器发生故障，Marathon会将其重新安排在其他从属节点上。 只有通过社区支持的组件才能使用资源指标进行自动扩展。6.应用程序滚动升级，回滚k8s : 在deployment中有滚动升级和回滚的策略。可以设置pod最大数量。marathon: 部署支持应用程序的滚动升级。失败的升级可以使用回滚更改的更新部署进行修复。7.健康检查k8s:健康检查有两种：活跃（即应用程序响应）和准备（应用程序响应，但正在忙着准备，还没有能够服务）。marathon:运行状况检查可以指定为针对应用程序的任务运行。健康检查请求可用于许多协议，包括HTTP，TCP和其他协议。8.存储k8s:两个存储API：第一个提供个人存储后端的抽象（例如NFS，AWS EBS，Ceph，Flocker）。 第二个提供存储资源请求的抽象，这可以用不同的存储后端来实现。 修改集群节点上Docker守护进程使用的存储资源需要暂时从集群中删除该节点。 Kubernetes提供了几种类型的持续卷，支持块或文件。 例子包括iSCSI，NFS，FC，亚马逊网络服务，Google云端平台和微软Azure。 emptyDir卷是非持久性的，可以用来读取和写入容器的文件。mesos/marathon:本地持久性卷（测试版）支持有状态的应用程序，如MySQL。 需要时，可以使用相同的卷在同一节点上重新启动任务。 外部存储（如Amazon EBS）的使用也在测试阶段。 目前，使用外部卷的应用程序只能缩放到一个实例，因为卷一次只能附加到一个任务。9.网络k8s:网络模型是一个扁平的网络，使所有的pod互相通信。 网络策略指定pod如何相互通信。 平面网络通常作为overlay来实现。 该模型需要两个CIDR：一个从中获取IP地址，另一个用于服务。mesos/marathon:网络可以在主机模式或网桥模式下进行配置。 在主机模式下，主机端口由容器使用。 这可能会导致任何给定主机上的端口冲突。 在桥接模式下，容器端口使用端口映射桥接到主机端口。 主机端口可以在部署时动态分配。10.服务发现k8s:可以使用环境变量或DNS来找到服务。 运行pod时，Kubelet会添加一组环境变量。 Kubelet支持简单的{SVCNAME\_SERVICE\_HOST}和{SVCNAME\_SERVICE\_PORT}变量，以及Docker链接兼容变量。 DNS服务器可作为附件使用。 对于每个Kubernetes服务，DNS服务器创建一组DNS记录。 在整个群集中启用DNS后，pod将能够使用自动解析的服务名称。marathon:服务可以通过“命名VIP”发现，它们是与IP和端口关联的DNS记录。 服务由Mesos-DNS自动分配DNS记录。 可以创建一个可选的命名VIP; 通过VIP的请求是负载平衡的。11.性能和节点支持k8s:  Kubernetes可扩展到5,000个节点的集群。可以集群联邦来扩展超出此限制。mesos/marathon: Mesos的2层体系结构（包括Marathon）非常具有可扩展性。据Digital Ocean介绍，Mesos和Marathon集群已经扩展到10,000个节点。优缺点k8s:各种各样的存储选项，包括本地SAN和公共云。 基于在Google上运行Linux容器的丰富经验。 在组织中更频繁地部署。 Kubernetes也得到来自Google（GKE）和RedHat（OpenShift）的企业支持。 容器编排工具中最大的社区。 超过50,000个提交者和1200个贡献者。mesos/marathon: Mesos + Marathon上的外部存储，包括Amazon EBS在内。。 Mesos被Mesosphere所利用。 Mesosphere公司的DCOS产品主要由其创建者和唯一的商业发行Mesosphere支持。。 较小的社区。 超过12,000个提交者和240个贡献者。k8s缺乏单一的供应商控制,会使潜在客户的采购决策复杂化。社区包括Google，Red Hat和2000多位作者。（来源：CNCF）Kubernetes仅为容器编排而建造。它基于10多年在Google管理Linux容器的经验。Kubernetes 1.6可以扩展到5,000个节点的集群。超过5,000个节点的大规模可扩展性需要多个集群。mesos/marathon: 单一供应商控制可能会考虑错误修复的问责制，以及与功能开发更好的协调。。 2层架构允许部署其他框架（工作负载）。 例子包括Spark，Chronos和Redis。 一些组织，如苹果，彭博，Netflix等已经大规模地部署了超过10,000个节点的Mesos。 （来源：Mesosphere博客）共同特点开源项目。任何人都可以贡献，但是Kubernetes享有更多，更多元化的社区参与。网络功能，如负载均衡和DNS。记录和监视。 Kubernetes的外部工具包括Elasticsearch / Kibana（ELK），sysdig，cAdvisor，Heapster / Grafana / InfluxDB（参考：Kubernetes的记录和监测）。对于Mesos / Marathon，节点提供可以汇总的日志，可以使用外部工具进行监视。 （参考：为Mesos / Marathon监控日志记录和调试）可以克服Docker和Docker API的限制。自动缩放支持本地。使用单独的一套管理工具。 Kubernetes操作可以通过kubectl CLI和Kubernetes Dashboard来执行。 Mesos使用多种界面：Mesos CLI，Mesos UI，Marathon CLI，Marathon UI。对于Kubernetes和Mesos，Marathon，自己动手安装可能会很复杂。 Kubernetes的部署工具包括kubeadm，kops，kargo等。 Kubernetes部署指南中的更多详细信息。由于采用Marathon的2层架构，集群管理的Zookeeper设置，负载平衡的HA代理等，Mesos的安装过程可能非常复杂

