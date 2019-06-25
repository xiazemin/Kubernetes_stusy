Kubernetes可以在多种平台运行，从笔记本电脑，到云服务商的虚拟机，再到机架上的裸机服务器。要创建一个Kubernetes集群，根据不同场景需要做的也不尽相同，可能是运行一条命令，也可能是配置自己的定制集群。这里我们将引导你根据自己的需要选择合适的解决方案。

## 选择正确的解决方案

如果你只是想试一试Kubernetes，我们推荐基于Docker的本地方案。

基于Docker的本地方案是众多能够完成快速搭建的本地集群方案中的一种，但是局限于单台机器。

当你准备好扩展到多台机器和更高可用性时，托管解决方案是最容易搭建和维护的。

全套云端方案 只需要少数几个命令就可以在更多的云服务提供商搭建Kubernetes。

定制方案 需要花费更多的精力，但是覆盖了从零开始搭建Kubernetes集群的通用建议到分步骤的细节指引。

### 本地服务器方案

本地服务器方案再一台物理机上创建拥有一个或者多个Kubernetes节点的单机集群。创建过程是全自动的，且不需要任何云服务商的账户。但是这种单机集群的规模和可用性都受限于单台机器。

本地服务器方案有：

* 本地Docker（上手建议）
* Vagrant \(任何支持Vagrant的平台：Linux，MacOS，或者Windows。\)
* 无虚拟机本地集群 \(Linux\)

### 托管方案

Google Container Engine 提供创建好的Kubernetes集群。

### 全套云端方案

以下方案让你可以通过几个命令就在很多IaaS云服务中创建Kubernetes集群，并且有很活跃的社区支持。

* GCE
* AWS
* Azure

### 定制方案

Kubernetes可以在2云服务提供商和裸机环境运行，并支持很多基本操作系统。

如果你再如下的指南中找到了符合你需要的，可直接使用。某些指南可能有些过时，但是比起从零开始还是有不少参考价值。如果你确实因为特殊原因或因为想了解底层原理，想要从  
零开始搭建，可以试试参考从零开始指南。

如果你对在新的平台支持Kubernetes感兴趣，可以看看我们的写新方案的建议。

### 云

以下是上文没有列出的云服务商或云操作系统支持的方案。

* AWS + coreos
* GCE + CoreOS
* AWS + Ubuntu
* Joyent + Ubuntu
* Rackspace + CoreOS

### 私有虚拟机

* Vagrant（采用CoreOS和flannel）
* CloudStack（采用Ansible，CoreOS和flannel）
* Vmware（采用Debian）
* juju.md（采用Juju，Ubuntu和flannel）
* Vmware（采用CoreOS和flannel）
* libvirt-coreos.md（采用CoreO）
* oVirt
* libvirt（采用Fedora和flannel）
* KVM（采用Fedora和flannel）

### 裸机

* Offline（无需互联网，采用CoreOS和flannel）
* fedora/fedora\_ansible\_config.md
* Fedora单节点
* Fedora多节点
* Centos
* Ubuntu
* Docker多节点

### 集成

Kubernetes on Mesos（采用GCE）

## Table of Solutions

以下用表格形式列出上面的所有方案。

| **IaaS Provider** | **Config.Mgmt** | **OS** | **Networking** | **Docs** | **Conforms** |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| GKE |  |  | GCE | [docs](https://cloud.google.com/container-engine) | \[✓\]\[3\] |  |
| Vagrant | Saltstack | Fedora | flannel | docs | \[✓\]\[2\] |  |
| GCE | Saltstack | Debian | GCE | docs | \[✓\]\[1\] |  |
| Azure | CoreOS | CoreOS | Weave | docs |  |  |
| Docker Single Node | custom | N/A | local | docs |  |  |
| Docker Multi Node | Flannel | N/A | local | docs |  |  |
| Bare-metal | Ansible | Fedora | flannel | docs |  |  |
| Digital Ocean | custom | Fedora | Calico | docs |  |  |
| Bare-metal | custom | Fedora | _none_ | docs |  |  |
| Bare-metal | custom | Fedora | flannel | docs |  |  |
| libvirt | custom | Fedora | flannel | docs |  |  |
| KVM | custom | Fedora | flannel | docs |  |  |
|  |  |  |  |  |  |  |

| Mesos/Docker | custom | Ubuntu | Docker | docs |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Mesos/GCE |  |  |  | docs |  |  |
| AWS | CoreOS | CoreOS | flannel | docs |  |  |
| GCE | CoreOS | CoreOS | flannel | docs |  |  |
| Vagrant | CoreOS | CoreOS | flannel | docs |  |  |
| Bare-metal \(Offline\) | CoreOS | CoreOS | flannel | docs |  |  |
| Bare-metal | CoreOS | CoreOS | Calico | docs |  |  |
| CloudStack | Ansible | CoreOS | flannel | docs |  |  |
| Vmware |  | Debian | OVS | docs |  |  |
| Bare-metal | custom | CentOS | _none_ | docs |  |  |
| AWS | Juju | Ubuntu | flannel | docs |  |  |
| OpenStack/HPCloud | Juju | Ubuntu | flannel | docs |  |  |
| Joyent | Juju | Ubuntu | flannel | docs |  |  |
| AWS | Saltstack | Ubuntu | OVS | docs |  |  |
| Azure | Saltstack | Ubuntu | OpenVPN | docs |  |  |
| Bare-metal | custom | Ubuntu | Calico | docs |  |  |
| Bare-metal | custom | Ubuntu | flannel | docs |  |  |
| Local |  |  | _none_ | docs |  |  |

| libvirt/KVM | CoreOS | CoreOS | libvirt/KVM | docs |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| oVirt |  |  |  | docs |  |  |
| Rackspace | CoreOS | CoreOS | flannel | docs |  |  |
| any | any | any | any | docs |  |  |

注意：以上表格按照支持级别和测试及使用的版本进行排序。

表格中列说明：

* **IaaS Provider**
  是指提供Kubernetes运行环境的虚拟机或物理机（节点）资源的提供商。
* **OS**
  是指节点上运行的基础操作系统。
* **Config. Mgmt**
  是指节点上安装和管理Kubernetes软件的的配置管理系统。
* **Networking**
  是指实现网络模型的软件。
  _none_
  表示只支持一个节点，或支持单物理节点 上的虚拟机节点。
* **Con**
  **formance**
  表示使用该种配置创建的集群是否通过了项目一致性测试，支持

Kubernetes v1.0.0的API和基本特性。

* Support Levels（支持级别）
* **Project**
  ：Kubernetes贡献者们经常使用该配置，所以通常最新的版本可使用。
* **C**
  **ommercial**
  ：某些厂商负责在自己的平台支持。
* **Community**
  ：在社区中有活跃支持，但可能最新版本不适用。
* **Inac**
  **tive**
  : 对于初次使用Kubernetes的用户不推荐，并且有可能在将来被移除。
* **Notes**
  说明，比如适用的Kubernetes版本。



