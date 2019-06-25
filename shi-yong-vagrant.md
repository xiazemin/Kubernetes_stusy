使用Vagrant（和VirtualBox）运行Kubernetes是在本地机器（Linux，Mac OS X）进行运行/测试/开发的简单方法。



预备知识

1. 从http://www.vagrantup.com/downloads.html下载最新版 Vagrant &gt;=1.6.2



2. 下载以下中的一项：



i. 从http://www.vagrantup.com/downloads.html下载4.3.28版本的VirtualBox



ii. 版本号&gt;=5的VMWare Fusion和合适的Vagrant VMWare Fusion provider



iii. 版本号&gt;=9的VMWare Workstation以及Vagrant VMWare Fusion provider



iv. 版本号&gt;=9的Parallels Desktop以及Vagrant Parallels provider



v. 支持硬件虚拟化并且带有 libvirt 的KVM。Vagrant-libvirt，为fedora提供官方的rpm，可以使用 yum install vagrant-libvirt 命令安装



设置

简单运行下列代码就能够设置一个集群



export KUBERNETES\_PROVIDER=vagrant

curl -sS https://get.k8s.io \| bash

或者，你也可以下载Kubernetes发行版、解压归档文件。打开终端运行命令启动本地集群：



cd kubernetes

export KUBERNETES\_PROVIDER=vagrant

./cluster/kube-up.sh

环境变量KUBERNETES\_PROVIDER用来告诉所有不同的集群管理脚本该使用哪一个脚本管理器（比如Vagrant）。如果你忘记设置这个变量，默认假设你运行Google Compute Engine上运行k8s。



默认情况下，Vagrant会创建一个单独的master VM（被称为kubernetes-master），以及一个节点VM（被称为kubernetes-minion-1）。每个VM会占用1G内存，所以确保你有至少2G-4G

的空余内存（以及合适的空闲硬盘空间）。



Vagrant会提供集群中每台机器运行Kuberbetes所有必须的组件。每台机器会花费几分钟完成初始化设置。



如果你下载了多个Vagrant provider，Kubernetes通常会选择最恰当的那个。但是，你可以通过设置环境变量VAGRANT\_DEFAULT\_PROVIDER的值来让Kubernetes使用哪个Vagrant

provider:



export VAGRANT\_DEFAULT\_PROVIDER=parallels

export KUBERNETES\_PROVIDER=vagrant

./cluster/kube-up.sh

默认情况下每个集群中的VM运行Fedora系统。



通过下面的命令连接master或任意节点：



vagrant ssh master

vagrant ssh minion-1

如果你运行超过一个节点，可以通过下面命令连接其它节点：



vagrant ssh minion-2

vagrant ssh minion-3

集群中的每个节点会下载docker daemon和kubelet。



The master node instantiates the Kubernetes master components as pods on the

machine.\( 这句翻译待讨论 \)



master节点会实例化Kubernetes master 组件作为 pods 运行在机器上。



查看kubernetes-master上的服务状态或者日志：



\[vagrant@kubernetes-master ~\] $ vagrant ssh master

\[vagrant@kubernetes-master ~\] $ sudo su

\[root@kubernetes-master ~\] $ systemctl status kubelet

\[root@kubernetes-master ~\] $ journalctl -ru kubelet

\[root@kubernetes-master ~\] $ systemctl status docker

\[root@kubernetes-master ~\] $ journalctl -ru docker

\[root@kubernetes-master ~\] $ tail -f /var/log/kube-apiserver.log

\[root@kubernetes-master ~\] $ tail -f /var/log/kube-controller-manager.log

\[root@kubernetes-master ~\] $ tail -f /var/log/kube-scheduler.log

查看任意节点的服务状态：



\[vagrant@kubernetes-master ~\] $ vagrant ssh minion-1

\[vagrant@kubernetes-master ~\] $ sudo su

\[root@kubernetes-master ~\] $ systemctl status kubelet

\[root@kubernetes-master ~\] $ journalctl -ru kubelet

\[root@kubernetes-master ~\] $ systemctl status docker

\[root@kubernetes-master ~\] $ journalctl -ru docker

使用Vagrant与kubernetes集群交互

启动好你的kubernetes集群后，你可以使用常用的Vagrant命令管理集群中的节点。



源代码改变后推送更新到Kubernetes代码：



./cluster/kube-push.sh

暂停与重启集群：



vagrant halt

./cluster/kube-up.sh

删除集群：



vagrant destroy

一旦你的Vgrant机器运行并且提供相应资源，第一件要做的事情就是检查能否使用kubectl.sh脚本。



你可能需要先编译二进制文件，可以使用make命令：



$ ./cluster/kubectl.sh get nodes

NAME LABELS

10.245.1.4 &lt;none&gt;

10.245.1.5 &lt;none&gt;

10.245.1.3 &lt;none&gt;



验证你的master

当我们使用vagrant provider管理Kubernetes时，cluster/kubectl.sh脚本会创建证书保存在 home 或 〜 （ps：Linux中使用 〜 表示当前用户的家目录）目录下的.kubernetes\_vagrant\_auth文件中，这样以后就不会出现验证的提示了。



cat ~/.kubernetes\_vagrant\_auth

{ "User": "vagrant",

"Password": "vagrant",

"CAFile": "/home/k8s\_user/.kubernetes.vagrant.ca.crt",

"CertFile": "/home/k8s\_user/.kubecfg.vagrant.crt",

"KeyFile": "/home/k8s\_user/.kubecfg.vagrant.key"

}

现在你可以使用cluster/kubectl.sh脚本了。例如使用下面的命令列出已经启动的节点：



./cluster/kubectl.sh get nodes

运行容器

你的集群已经运行起来了，你可以列出集群中的节点：



$ ./cluster/kubectl.sh get nodes

NAME LABELS

10.245.2.4 &lt;none&gt;

10.245.2.3 &lt;none&gt;

10.245.2.2 &lt;none&gt;

现在开始运行一些容器吧！



现在你可以用cluster/kube-\*.sh的任何命令来与你的VMs交互。启动容器之前并没有 pods 、 service 和 replication controller ：



$ ./cluster/kubectl.sh get pods

NAME READY STATUS RESTARTS AGE

$ ./cluster/kubectl.sh get services

NAME LABELS SELECTOR IP\(S\) PORT\(S\)

$ ./cluster/kubectl.sh get replicationcontrollers

CONTROLLER CONTAINER\(S\) IMAGE\(S\) SELECTOR REPLICAS

启动一个运行nginx、使用复制控制器并且副本数为3的容器：



$ ./cluster/kubectl.sh run my-nginx --image=nginx --replicas=3 --port=80

（此时）列出pods，会看到3个容器已经被启动了，并且处于等待状态：



$ ./cluster/kubectl.sh get pods

NAME READY STATUS RESTARTS AGE

my-nginx-5kq0g 0/1 Pending 0 10s

my-nginx-gr3hh 0/1 Pending 0 10s

my-nginx-xql4j 0/1 Pending 0 10s

你需要等待（Vagrant）分配好资源，这时可以通过命令来监测节点：



20161107171616



 



下载好docker nginx镜像后容器就会启动，通过下面的命令查看：



20161107171624



 



这时再次列举pods、服务和复制控制器，会看到：



$ ./cluster/kubectl.sh get pods

NAME READY STATUS RESTARTS AGE

my-nginx-5kq0g 1/1 Running 0 1m

my-nginx-gr3hh 1/1 Running 0 1m

my-nginx-xql4j 1/1 Running 0 1m

$ ./cluster/kubectl.sh get services

NAME LABELS SELECTOR IP\(S\) PORT\(S\)

$ ./cluster/kubectl.sh get replicationcontrollers

CONTROLLER CONTAINER\(S\) IMAGE\(S\) SELECTOR REPLICAS

my-nginx my-nginx nginx run=my-nginx 3

我们没有启动任何服务，因此列举服务是什么也没有。但是我们看到3个副本正常显示。查看guestbook了解如何创建服务。现在你已经可以尝试增加或减少副本了：



$ ./cluster/kubectl.sh scale rc my-nginx --replicas=2

$ ./cluster/kubectl.sh get pods

NAME READY STATUS RESTARTS AGE

my-nginx-5kq0g 1/1 Running 0 2m

my-nginx-gr3hh 1/1 Running 0 2m

祝贺你！



解决常见问题

我一直在下载（很大的）box（ps：box指vagrant封装好的vm，可以下载直接使用，不用自己再从iso安装）



Vagrantfile默认从S3下载box。当调用kube-up.sh脚本是你可以提供name和可选的连接作为参数来修改下载站点：



export KUBERNETES\_BOX\_NAME=choose\_your\_own\_name\_for\_your\_kuber\_box

export KUBERNETES\_BOX\_URL=path\_of\_your\_kuber\_box

export KUBERNETES\_PROVIDER=vagrant

./cluster/kube-up.sh

我只是创建了集群，但是遇到授权错误很可能你正试图连接的集群中的~/.kubernetes\_vagrant\_auth文件不正确：



rm ~/.kubernetes\_vagrant\_auth

使用kubectl.sh创建正确的证书：



cat ~/.kubernetes\_vagrant\_auth

{

"User": "vagrant",

"Password": "vagrant"

}

我已经创建了集群，但是看不到运行的容器！



如果是你第一次创建集群，每个节点上的kubelet会制定一个docker下载镜像和依赖镜像的时间表。这可能会占用很多时间并且推迟初始化pod、分配资源。



我想修改Kubernetes代码



你可以设置集群来 hacking ，参考vagrant developer guide



已经运行Vagrant up命令，但是节点没有生效



（ vagrant ssh minion-1 ）连接到一个节点查看日志和salt minion日志（ sudo cat/var/log/salt/minion ）



我想修改节点数



我们可以控制通过主机上环境变量NUM\_MINIONS实例化的节点的数量。如果你计划使用副本，我们强烈支持你启动足够的节点来满足最大可能的副本大小。如果你不打算使用副本，你可以运行单个节点来节省一些系统资源。只需要设置NUM\_MINIONS为1：



export NUM\_MINIONS=1

我想给我的VMs分配更多内存



使用环境变量KUBERNETES\_MEMORY控制分配给虚拟机的内存大小。只需要把它设置为你想要的大小，比如：



export KUBERNETES\_MEMORY=2048

如果你想要更细粒度的控制内存，你可以单独设置master和节点的内存大小，比如：



export KUBERNETES\_MASTER\_MEMORY=1536

export KUBERNETES\_MINION\_MEMORY=2048

运行vagrant使VM待机，但是失效



vagrant suspend命令很可能弄混了网络。这种情况下不支持这条命令。



我想通过vagrant通过NFS同步文件



可以设置环境变量KUBERNETES\_VAGRANT\_USE\_NFS的值为true来保证vagrant使用nfs同步虚拟机的文件夹。nfs比virtualbox和vmware的 共享文件夹 都要快并且不需要额外的设置。可以阅读vagrant docs获取在主机上配置nfs的细节。这项设置不会影响libvirt provider，它默认使用nfs：



export KUBERNETES\_VAGRANT\_USE\_NFS=true

