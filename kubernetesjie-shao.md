1.背景介绍

　　云计算飞速发展

　　　　- IaaS

　　　　- PaaS

　　　　- SaaS

　　Docker技术突飞猛进

　　　　- 一次构建，到处运行

　　　　- 容器的快速轻量

　　　　- 完整的生态环境

2.什么是kubernetes

　　首先，他是一个

全新的基于容器技术的分布式架构领先方案

。Kubernetes\(k8s\)是Google开源的容器集群管理系统（谷歌内部:Borg）。在Docker技术的基础上，为容器化的应用提供部署运行、资源调度、服务发现和动态伸缩等一系列完整功能，提高了大规模容器集群管理的便捷性。

　　Kubernetes是一个完备的分布式系统支撑平台，

具有完备的集群管理能力，多扩多层次的安全防护和准入机制、多租户应用支撑能力、透明的服务注册和发现机制、內建智能负载均衡器、强大的故障发现和自我修复能力、服务滚动升级和在线扩容能力、可扩展的资源自动调度机制以及多粒度的资源配额管理能力

。同时Kubernetes提供完善的管理工具，涵盖了包括开发、部署测试、运维监控在内的各个环节。

Kubernetes中，Service是分布式集群架构的核心，一个Service对象拥有如下关键特征：

* 拥有一个唯一指定的名字
* 拥有一个虚拟IP（Cluster IP、Service IP、或VIP）和端口号
* 能够体统某种远程服务能力
* 被映射到了提供这种服务能力的一组容器应用上

　　Service的服务进程目前都是基于Socket通信方式对外提供服务，比如Redis、Memcache、MySQL、Web Server，或者是实现了某个具体业务的一个特定的TCP Server进程，虽然一个Service通常由多个相关的服务进程来提供服务，每个服务进程都有一个独立的Endpoint（IP+Port）访问点，但Kubernetes能够让我们通过服务连接到指定的Service上。有了Kubernetes内奸的透明负载均衡和故障恢复机制，不管后端有多少服务进程，也不管某个服务进程是否会由于发生故障而重新部署到其他机器，都不会影响我们队服务的正常调用，更重要的是这个Service本身一旦创建就不会发生变化，意味着在Kubernetes集群中，我们不用为了服务的IP地址的变化问题而头疼了。

　　容器提供了强大的隔离功能，所有有必要把为Service提供服务的这组进程放入容器中进行隔离。为此，Kubernetes设计了Pod对象，将每个服务进程包装到相对应的Pod中，使其成为Pod中运行的一个容器。为了建立Service与Pod间的关联管理，Kubernetes给每个Pod贴上一个标签Label，比如运行MySQL的Pod贴上name=mysql标签，给运行PHP的Pod贴上name=php标签，然后给相应的Service定义标签选择器Label Selector，这样就能巧妙的解决了Service于Pod的关联问题。

　　在集群管理方面，Kubernetes将集群中的机器划分为一个Master节点和一群工作节点Node，其中，在Master节点运行着集群管理相关的一组进程kube-apiserver、kube-controller-manager和kube-scheduler，这些进程实现了整个集群的资源管理、Pod调度、弹性伸缩、安全控制、系统监控和纠错等管理能力，并且都是全自动完成的。Node作为集群中的工作节点，运行真正的应用程序，在Node上Kubernetes管理的最小运行单元是Pod。Node上运行着Kubernetes的kubelet、kube-proxy服务进程，这些服务进程负责Pod的创建、启动、监控、重启、销毁以及实现软件模式的负载均衡器。

