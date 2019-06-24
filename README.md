# Introduction

![](https://pic3.zhimg.com/80/v2-c2a31e2008835b2974170ad1dbac0d42_hd.jpg)

容器和虚拟机的对比

Docker技术的三大核心概念，分别是：

* **镜像（Image）**
* **容器（Container）**
* **仓库（Repository）**

就在Docker容器技术被炒得热火朝天之时，大家发现，如果想要将Docker应用于具体的业务实现，是存在困难的——编排、管理和调度等各个方面，都不容易。于是，人们迫切需要一套管理系统，对Docker及容器进行更高级更灵活的管理。

就在这个时候，K8S出现了。

**K8S，就是基于容器的集群管理平台，它的全称，是kubernetes。**

K8S并不是一件全新的发明。它的前身，是Google自己捣鼓了十多年的**Borg系统**。

  


K8S是2014年6月由Google公司正式公布出来并宣布开源的。

  


同年7月，微软、Red Hat、IBM、Docker、CoreOS、 Mesosphere和Saltstack 等公司，相继加入K8S。  


  


之后的一年内，VMware、HP、Intel等公司，也陆续加入。  


  


2015年7月，Google正式加入OpenStack基金会。与此同时，Kuberentes v1.0正式发布。

  


目前，kubernetes的版本已经发展到V1.13。

  


  


K8S的架构，略微有一点复杂，我们简单来看一下。

  


一个K8S系统，通常称为一个**K8S集群（Cluster）**。

  


这个集群主要包括两个部分：

* **一个Master节点（主节点）**
* **一群Node节点（计算节点）**

  


  


![](https://pic4.zhimg.com/80/v2-466804fc47bd2e939e0413d9c32170af_hd.jpg)

  


  


一看就明白：Master节点主要还是负责管理和控制。Node节点是工作负载节点，里面是具体的容器。

  


深入来看这两种节点。

  


首先是**Master节点**。

  


  


![](https://pic2.zhimg.com/80/v2-7fa63b292368c8f21bd4582861a6983d_hd.jpg)

  


  


Master节点包括API Server、Scheduler、Controller manager、etcd。

  


API Server是整个系统的对外接口，供客户端和其它组件调用，相当于“营业厅”。

  


Scheduler负责对集群内部的资源进行调度，相当于“调度室”。

  


Controller manager负责管理控制器，相当于“大总管”。

  


然后是**Node节点**。

  


  


![](https://pic4.zhimg.com/80/v2-8cb338cd8923fa0e6857f45facc8f00f_hd.jpg)

  


  


Node节点包括Docker、kubelet、kube-proxy、Fluentd、kube-dns（可选），还有就是**Pod**。

  


Pod是Kubernetes最基本的操作单元。一个Pod代表着集群中运行的一个进程，它内部封装了一个或多个紧密相关的容器。除了Pod之外，K8S还有一个**Service**的概念，一个Service可以看作一组提供相同服务的Pod的对外访问接口。

