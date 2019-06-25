Cloud Provider

Kubernetes有一个概念叫Cloud Provider，是指一个提供管理TCP负载均衡，节点（实例）和路由器接口的模块。 pkg/cloudprovider/cloud.go 里具体定义了这个接口。 当然，你也可以不用实现这个Cloud Provider去新建一个自定义的聚群（例如，使用裸机集群）。取决于不同部件是如何设置的，并不是所有接口都需要实现的。



node（节点）

你可以使用虚拟机或物理机。

为了运行本指南给出的例子和测试用例，你最好有4个节点以上。

虽说很多入门指南讲主节点和普通节点做了区分，但严格意义上讲，这不是必要的。

节点需要运行在x86\_64的Linux系统上。当然也可能运行在其他的系统和CPU架构上，但这个指南不会提供相关的帮助。

对于一个拥有10个以上节点的集群来说，Apiserver和etcd可以一起安装在一台1核和1GB内存的机子上。

你可以给其他节点可以分配合理的内存和CPU内核。不是所有节点需要同样的配置。

网络

Kubernetes有一个独特的网络模型.



Kubernetes给每一个pod分配IP地址。当你新建一个集群，为了保证Pod获得IP地址，你需要给Kubernetes分配一个IP地址池。最简单的做法是每当节点与节点之间的通信可以以一下两种方式实现：



（1）配置网络完成Pod的IP地址路由



因为一切从头开始，所以难度会大一些。

Google Compute Engine \(GCE\) 和 AWS会指导如何使用这种方式

需要编程配置路由器和交换机去实现Pod的IP地址路由。

可在Kubernetes环境外配置或者通过在Cloud Provider模块的“路由”接口里实现。

通常情况下，提供最优网络性能。

（2）建立一个拓扑网络



较容易建立

因为数据流量被封装，所以每个Pod的IP地址是可以路由的。

例如：

Flannel

Weave

Open vSwitch \(OVS\)

不需要CLoud Provider模块里的“路由”部分

较为不太理想的性能（具体的性能弱化取决于你的实际情况）

你需要为Pod所需要的IP地址选一个IP地址范围。



（3）一些可选择配置方式：



GCE：每一个项目有一个自己的IP子网“10.0.0.0/8”。项目中的每一个Kubernetes集群从中获得一个“/16”的子网。每一个节点从’/16’的子网里获IP地址。

AWS：在一个组织内使用一个VPC。从中分配地址给每一个聚群或者使用给不同的集群分配不同的VPC。

暂不支持支持IPv6

（4）给每一的node的Pod地址分配一个CIDR子网或者一个



你总共需要max-pods-per-node \* max-number-of-nodes个IP地址。一个“/24” 如果缺少IP地址，一个“/26”（62个节点）或者“/27”（30个节点）也能满足。

例如，使用“10.10.0.0/16” “10.10.0.0/24” “10.10.255.0/24”

需要路由设置或连接到拓扑网络

Kubernetes 会给每个service分配一个IP地址。 但是service的地址并不一定是可路由的。当数据离开节点时，kube-proxy需要将Service IP地址翻译成Pod IP地址。因此，你需要给Service也分配一个地址段。这个网段叫做“SERVICE\_CLUSTER\_IP\_RANGE”。例如，你可以这样设置“SERVICE\_CLUSTER\_IP\_RANGE=”10.0.0.0/16”，这样的话就会允许65534个不同的Service同时运行。请注意，你可以调整这个IP地址段。但不允许在Service和Pod运行的时候移除这个网段。



同样，你需要为主节点选一个静态IP地址。 －命名这个IP地址为“MASTER\_IP”。 －配置防火墙，从而允许访问apiserver端口80和443。 －使用sysctl设置”net.ipv4.ip\_forward = 1“从而开启IPv4 forwarding。



集群命名

为你的集群选个名字。要选一个简短不会和其他服务重复的名字。



用kubectl来访问不同的集群。比如当你想在其他的区域测试新的Kubernetes版本。

Kubernetes集群可以建立一个Cloud Provider资源（例如，AWS ELB）。所以不同的集群要能区分他们之间的相关资源，这个名字叫做“CLUSTERNAME”。

软件安装包

你需要以下安装包



etcd

以下容器二选一:

docker

rkt

Kubernetes

kubelet

kube-proxy

kube-apiserver

kube-controller-manager

kube-scheduler

下载和解压缩Kubernetes安装

Kubernets安装版本包包含所有Kuberentes的二进制发行版本和所对应的etcd。你可使直接使用这个二进制发行版本（推荐）或者按照开发者文档说明编译这些Kubernetes的二进制文件。 本指南只讲述了如何直接使用二进制发行版本。 下载最新安装版本并解压缩。之后找到你下载“./kubernetes/server/kubernetes-server-linux-amd64.tar.gz”的路径， 并解压缩这个压缩包。然后在其中找到“./kubernetes/server/bin”文件夹。里面有所所需的可运行的二进制文件。



选择安装镜像

在容器外运行docker，kuberlet和kube-proxy，就像你运行任何后台系统程序。所以你只需要这些基本的二进制运行文件。etcd， kube-apiserver， kube-controller-manager和kubescheduler，我们建议你在容器里运行etcd， kube-apiserver， kube-controller-manager和kube-scheduler。所以你需要他们的容器镜像。 你可以选择不同的Kubernetes镜像：



你可以使用Google Container Registry \(GCR\)上的镜像文件：



例如“gcr.io/google\_containers/hyperkube:$TAG”。这里的“TAG”是指最新的发行版本的标示。这个表示可以从最新发行说明找到。

确定$TAG和你使用的kubelet，kube-proxy的标签是一致的。

hyperkube是一个集成二进制运行文件

你可以使用“hyperkube kubelet …”来启动kubelet ，用“hyperkube apiserver …”运行apiserver， 等等。

生成你自己的镜像：



使用私有镜像服务器。

例如，可以使用“docker load -i kube-apiserver.tar”将“./kubernetes/server/bin/kubeapiserver.tar”文件转化成dokcer镜像。

之后可使用“docker images”来验证镜像是否从制定的镜像服务器加载成功。

对于etcd，你可以：



使用上Google Container Registry \(GCR\)的镜像，例如“gcr.io/google\_containers/etcd:2.0.12”

使用Docker Hub或着Quay.io上的镜像，例如“quay.io/coreos/etcd:v2.2.0”

使用你操作系统自带的etcd

自己编译

你可以使用这个命令行“cd kubernetes/cluster/images/etcd; make”

我们建议你使用Kubernetes发行版本里提供的etcd版本。Kubernetes发行版本里的二进制运行文件只是和这个版本的etcd测试过。你可以在“kubernetes/cluster/images/etcd/Makefile”里“ETCD\_VERSION”所对应的值找到所推荐的版本号。 接下来本文档会假设你已经选好镜像标示并设置了对应的环境变量。 设置好了最新的标示和正确的镜像服务器：



“HYPERKUBE\_IMAGE==gcr.io/google\_containers/hyperkube:$TAG”

“ETCD\_IMAGE=gcr.io/google\_containers/etcd:$ETCD\_VERSION”

安全模式

这里有两种主要的安全选项：



使用HTTP访问apiserver



配合使用防火墙。

这种方法比较易用。

使用HTTPS访问apiserver



配合电子证书和用户登录信息使用。

推荐使用

设置电子证书较为复杂。

如果要用HTTPS这个方式，你需要准备电子证书和用户登录信息。



准备安全证书

你需要准备多个证书：



主节点会是一个HTTPS服务器，所以需要一个证书。

如果Kuberlets需要通过HTTPS提供API服务时，这些kuberlets需要出示证书向主节点证明主从关系。

除非你决定要一个真正CA来生成这些证书的话，你需要一个根证书，并用这个证书来给主节点，kuberlet和kubectl的证书签名。



参见“cluster/gce/util.sh”脚本里的“create-certs”

并参见“cluster/saltbase/salt/generate-cert/make-ca-cert.sh”和cluster/saltbase/salt/generate-cert/make-cert.sh“

你需要修改以下部分\(我们之后也会用到这些参数的\)



”CA\_CERT“



放置在apiserver所运行的节点上，比如“/srv/kubernetes/ca.crt”。

“MASTER\_CERT”



用CA\_CERT来签名

放置在apiserver所运行的节点上，比如”/srv/kubernetes/server.crt”。

“MASTER\_KEY“



放置在apiserver所运行的节点上，比如”/srv/kubernetes/server.key“。

“KUBELET\_CERT”



可选配置

”KUBELET\_KEY“



可选配置

准备登录信息

管理员（及任何用户）需要：



对应验证身份的令牌或密码。

令牌可以是长字符串，比如 32个字符

可参见”TOKEN=$\(dd if=/dev/urandom bs=128 count=1 2&gt;/dev/null \| base64 \| tr -d”=+/” \| dd bs=32 count=1 2&gt;/dev/null\)““

你的令牌和密码需要保存在apiserver上的一个文件里。本指南使用这个文件”/var/lib/kubeapiserver/known\_tokens.csv“。文件的具体格式在认证文档里描述了。 至于如何把登录信息分发给用户，Kubernetes是将登录信息放入kubeconfig文件里。



管理员可以按如下步骤创建kubeconfig文件：



如果你已经在非客制化的集群上运行过Kubernetes\(例如，按照入门指南架设过Kubernetes\)，那么你已经有“$HOME/.kube/config\`”文件了。

你需要在kuberconfig文件里添加证书，密钥和主节点IP：

1、如果你选择了“firewall-only”的安全设置，你需要按如下设置apiserver：

2、“kubectl config set-cluster $CLUSTER\_NAME –server=http://$MASTER\_IP –insecure-skip-tls-verify=true”

3、否则，按如下设置你的apiserver的IP，证书，用户登录信息：

4、“kubectl config set-cluster $CLUSTER\_NAME –certificate-authority=$CA\_CERT –embed-certs=true –server=https://$MASTER\_IP”

5、“kubectl config set-credentials $USER –client-certificate=$CLI\_CERT –clientkey=$CLI\_KEY –embed-certs=true –token=$TOKEN”

6、设置你的集群为缺省集群：

7、“kubectl config set-context $CONTEXT\_NAME –cluster=$CLUSTER\_NAME –user=$USER”

8、“kubectl config use-context $CONTEXT\_NAME”

接下来，为kubelets和kube-proxy准备kubeconfig文件。至于要准备多少不同的文件，这里有几个选项：



1. 使用和管理员同样的登陆账号



这是最简单的建设方法。

2. 所有的kubelet使用同一个令牌和kubeconfig文件，另外一套给所有的kube-proxy使用，在一套给管理员使用。



这个设置和GCE的配置类似。

3. 为每一个kubelet，kube-proxy和管理员准备不同的登陆账号。



这个配置在实现中，目前还不支持。

为了生成这个文件，你可以参照“cluster/gce/configure-vm.sh”中的代码直接从“$HOME/.kube/config”拷贝过去或者参考以下模版：



apiVersion: v1

kind: Config

users:

- name: kubelet

user:

token: ${KUBELET\_TOKEN}

clusters:

- name: local

cluster:

certificate-authority-data: ${CA\_CERT\_BASE64\_ENCODED}

contexts:

- context:

cluster: local

user: kubelet

name: service-account-context

current-context: service-account-context

把kubeconfig文件放置到每一个节点上。本章节之后的事例会假设kubeconfig文件已经放置在“/var/lib/kube-proxy/kubeconfig”和“/var/lib/kubelet/kubeconfig”里。



在节点上配置和安装基础软件

这个章节讨论的是如歌配置Kubernetes节点。 你应该在每个节点运行三个后台进程：



docker or rkt

kubelet \*kube-proxy

Docker容器

对最低Docker版本的要求是随着kubelet的版本变化的。最新的稳定版本通常是个好选择。如果版本太低，Kubelet记录下警报并拒绝运行pod，所以你可以选择个版本试一下。



如果你之前安装Docker的节点没有Kubernetes相关的配置，你可能已经有Docker新建的网桥和iptables的规则。所以你或许希望在为Kubernetes配置Docker前根据以下命令移除之前的配置。



iptables -t nat -F

ifconfig docker0 down

brctl delbr docker0

如何配置Docker取决于你网络是基于routable-vip还是overlay-network。 这里有一些建议的Docker选项：



为每一个节点的CIDR网断建立你自己的网桥，命名为cbr0并为docker设置 –bridge=cbr0 。

配置 –iptables=false ，所以docker不会为host-port设置iptables\(这个控制在docker旧版本不够细致，以后会在新版本里修复\)

所以kube-proxy可以代替docker来设置iptables。



–ip-masq=false

如果你将PodIP设置为可路由寻址，你会希望将这个选项设置为false。否则，docker会将NodeIP重写为PodIP的源地址。

一些环境\(例如,GCE\)下需要伪装\(masquerade\)离开这个云环境的流量。这个配置是取决于具体的云环境的。

如果你在使用overlay网络，请参考其他资料。

–mtu=

但使用Flannel的时候，需要这个选项。 因为UDP包封装造成过大的数据包。

–insecure-registry $CLUSTER\_SUBNET

为链接没有SSL安全链接的私有registry。

你或许希望为Docker提高可以打开文件的数目：



DOCKER\_NOFILE=1000000 这里的设置取决于你的节点的操作系统。比如，GCE上基于Debian的发行版本使用 /etc/default/docker 这个配置文件。

在进行下一步安装前，可以参考Docker文档里的实例来确保docker在你的系统上正常工作。



rkt

rkt是类似Docker的技术。你只需要二选一安装Docker或者rkt。最低的版本是v0.5.6。



systemd是在节点上运行rkt必须的。与rkt v0.5.6所对应的最低版本是systemd 215。



rkt metadata service也是必须安装的，来支持rtk的网络部分。你可以用以下命令来运行rkt的metadata服务 sudo systemd-run rkt metadata-service



接下来你需要来设置kubelet的标记：



--container-runtime=rkt

kubelet

所有的节点都要运行kubelet。



可参考的参数：



如果选择HTTPS的安全配置：

–api-servers=https://$MASTER\_IP

–kubeconfig=/var/lib/kubelet/kubeconfig

否则，使用防火墙的安全配置：

–api-servers=http://$MASTER\_IP

–config=/etc/kubernetes/manifests

–cluster-dns= 是用来配置DNS服务器的地址\(参考Starting Addons.\)

–cluster-domain= 是为DNS集群地址使用的DNS域名前缀。

–docker-root=

–root-dir=

–configure-cbr0= \(参考之前的介绍\)

–register-node \(参考章节节点.\)

kube-proxy

所有的节点都要运行kube-proxy。\(并不一定要在主节点上运行kube-proxy，但最好还是与其它节点保持一致\) 可参考如何获得kubelet二进制运行包来获得kube-proxy二进制运行包。



可参考的参数



如果选择HTTPS的安全配置

–api-servers=https://$MASTER\_IP

–kubeconfig=/var/lib/kube-proxy/kubeconfig

否则，使用防火墙的安全配置：

–api-servers=http://$MASTER\_IP

网络

为了pod的网络通信，需要给每一个节点分配一个自己的CIDR网段。这个叫做 NODE\_X\_POD\_CIDR 。



需要给每一个节点新建一个叫 cbr0 网桥。网桥会在networking documentation里做详细介绍。约定俗成，$NODE\_X\_POD\_CIDR 里的第一个IP地址作为这个网桥的IP地址。这个地址叫做 NODE\_X\_BRIDGE\_ADDR 。比如， NODE\_X\_POD\_CIDR 是 10.0.0.0/16 ，那么 NODE\_X\_BRIDGE\_ADDR 是 10.0.0.1/16 。注意：这里用 /16 这个后缀是因为之后也会这么使用。



推荐的自动化步骤:

1. 在初始化的脚本里，设置kubelet的选项为 –configure-cbr0=true ，并重启kubelet服务。Kubelet会自动设置cobr0. 它会一直等待，直到节点controller正确设置Node.Spec.PodCIDR。因为你目前还没有设置好apiserver和节点controller，所以网桥不会马上完成设置。

人工步骤:

1. 设置kubelet的选项 –configure-cbr0=false ，并重启kubelet。

2. 新建网桥比如 brctl addbr cbr0 .

3. 设定合适的MTU

比如 ip link set dev cbr0 mtu 1460 \(注意: 真实的MTU值是由你的网络环境所决定的\)

4. 把集群网络加入网桥\(docker会连接在这个网桥的另一端\)。

比如 ip addr add $NODE\_X\_BRIDGE\_ADDR dev cbr0

5. 开启网桥

比如 ip link set dev cbr0 up

在你关闭了Docker的IP伪装的情况下，为了让pod之间相互通信，你可能需要为去往集群网络外的流量做目的IP地址伪装，例如：



iptables -t nat -A POSTROUTING ! -d ${CLUSTER\_SUBNET} -m addrtype ! --dst-type LOCAL -jMASQUERADE

这样会重写从PodIP到节点IP的数据流量的原地址。内核connection tracking会确保发向节点的回复能够到达pod。



注意: 需不需要IP地址伪装是视环境而定的。在一些环境下是不需要IP伪装的。例如GCE这样的环境从pod发出的数据是不允许发向Interent的，但如果在同一个GCE项目里是不会有问题的。



其他

如果需要，为你的系统安装包管理器开启自动升级。

为所有的节点设置日志轮询\(比如，使用logrotate\)。

建立liveness-monitoring \(比如，使用monit\)。

建立存储插件的支持\(可选\)

为可选的存储类型安装所需的客户端程序，比如为GlusterFS安装 glusterfsclient。

使用配置管理工具

之前架设服务器的步骤都是使用“传统”的系统管理方式。你可以尝试使用系统配置工具来自动化架设流程。你可以参考其他入门指南，比如使用Saltstack， Ansible， Juju和CoreOSCloud Config。



引导安装集群

通常情况下，基本的节点服务\(kubelet， kube-proxy和docker\)都是由传统的系统配置方式完成建立和管理的。其他的Kubernetes的相关部分都是由Kubernetes本身来完成配置和管理的：



配置和管理的选项在Pod spec\(yaml or json\)而不是/etc/init.d文件或systemd unit里定义的。

他们都是由Kubernetes而不是init来负责运行的。

etcd

你需要运行一个或多个etcd实例。



推荐方式: 运行一个etcd实例，将日志保存在类似RAID，GCE PD的永久存储空间上。

或者: 运行3个或者5个etcd实例。

日志可以保存在 Log can be written to non-durable storage because storage isreplicated.

运行一个apiserver，这个apiserver连接到其中一个etc实例上。 参见clustertroubleshooting

获取更多的有关集群可用性的信息。

启动一个etcd实例:



1.复制 cluster/saltbase/salt/etcd/etcd.manifest 2.做有必要的设置修改 3.将这个文件放到kubelet mainfest的文件夹中



Apiserver，Controller Manager和Scheduler

在主节点上，apiserver，controller manager，scheduler会运行在各自的pod里。



启动以上三个服务的步骤大同小异：



1. 从为pod所提供的template开始。



2. 设置 HYPERKUBE\_IMAGE 的值为选择安装镜像中所设置的值。



3. 可参考以下的template来决定你集群所需的选项。



4. 在 commands 列表里设置所需的运行选项（例如，$ARGN）。



5. 将完成的template放在kubelet manifest的文件夹内。



6. 验证pod是否运行。



Apiserver pod模版

{

"kind": "Pod",

"apiVersion": "v1",

"metadata": {

"name": "kube-apiserver"

},

"spec": {

"hostNetwork": true,

"containers": \[

{

"name": "kube-apiserver",

"image": "${HYPERKUBE\_IMAGE}",

"command": \[

"/hyperkube",

"apiserver",

"$ARG1",

"$ARG2",

...

"$ARGN"

\],

"ports": \[

{

"name": "https",

"hostPort": 443,

"containerPort": 443

},

{

"name": "local",

"hostPort": 8080,

"containerPort": 8080

}

\],

"volumeMounts": \[

{

"name": "srvkube",

"mountPath": "/srv/kubernetes",

"readOnly": true

},

{

"name": "etcssl",

"mountPath": "/etc/ssl",

"readOnly": true

}

\],

"livenessProbe": {

"httpGet": {

"path": "/healthz",

"port": 8080

},

"initialDelaySeconds": 15,

"timeoutSeconds": 15

}

}

\],

"volumes": \[

{

"name": "srvkube",

"hostPath": {

}

},

{

"name": "etcssl",

"hostPath": {

"path": "/etc/ssl"

}

}

\]

}

}

可选设置的apiserver的选项：



–cloud-provider= 参见 cloud providers

–

-cloud-config= 参见 cloud providers

如果你想在主节点运行proxy，你需要设置 —address=${MASTER\_IP} 或者 –bindaddress=127.0.0.1 和 –address=127.0.0.1 。

–cluster-name=$CLUSTER\_NAME

–service-cluster-ip-range=$SERVICE\_CLUSTER\_IP\_RANGE

–etcd-servers=http://127.0.0.1:4001

–tls-cert-file=/srv/kubernetes/server.cert

–tls-private-key-file=/srv/kubernetes/server.key

–admission-control=$RECOMMENDED\_LIST

参考 admission controllers.

只有当你相信你的集群用户可以使用root权限来运行pod时，开启这个选项 –allowprivileged=true ,

如果你是按照firewall-only的安全方式来配置的，你需要以下设置：



–token-auth-file=/dev/null

—insecure-bind-address=$MASTER\_IP

–advertise-address=$MASTER\_IP

如果你是按照HTTPS的安全方式来配置的，你需要以下设置：



–client-ca-file=/srv/kubernetes/ca.crt

–token-auth-file=/srv/kubernetes/known\_tokens.csv

–basic-auth-file=/srv/kubernetes/basic\_auth.csv

这个pod使用 hostPath 加载多个节点文件系统目录。这些加载的目录的用途是：



加载 /etc/ssl 目录可以允许apiserver找到SSL根证书，从而验证例如云服务提供商所提供的外部服务。

如果你不使用任何云服务提供商，你就不需要配置这里（比如，只使用物理裸机）。

加载 /srv/kubernetes 目录可以允许读取存储在节点磁盘上的证书和认证信息。

可选, 你也可以加在 /var/log 目录从而将日志记录在这个目录里\(没有在template里举例标明\)。

如果你想用类似journalctl的工具从根文件系统来访问日志的话，可以加载这个目录。 TODO 描述如何架设proxy-ssh。

Cloud Providers

Apiserver支持多个cloud providers。



–cloud-provider 选项的值可以

是 aws ， gce ， mesos ， openshift ， ovirt ， rackspace ， vagrant 或者不´未设置。

未设置选项可以用来设置物理裸机。

在这里添加新的IaaS。

一些cloud providers需要配置文件。这种情况下，你需要将配置文件放置在apiserver的镜像中或者通过 hostPath 来加载。



如果cloud providers需要配置文件，设置 —cloud-config= 这个选项。

aws ， gce ， mesos ， openshift ， ovirt 和 rackspace 会使用到这个选项。

你必须需要将配置文件放置在apiserver的镜像中或者通过 hostPath 来加载。

云配置文件的语法Gcfg。

AWS格式是用类型来定义的AWSCloudConfig。

其他的云服务提供商也有类似的对应文件。

比如在GCE里: 在这个文件找 gce.conf 字节。

Scheduler pod template

完成scheduler pod的template：



{

"kind": "Pod",

"apiVersion": "v1",

"metadata": {

"name": "kube-scheduler"

},

"spec": {

"hostNetwork": true,

"containers": \[

{

"name": "kube-scheduler",

"image": "$HYBERKUBE\_IMAGE",

"command": \[

"/hyperkube",

"scheduler",

"--master=127.0.0.1:8080",

"$SCHEDULER\_FLAG1",

...

"$SCHEDULER\_FLAGN"

\],

"livenessProbe": {

"httpGet": {

"host" : "127.0.0.1",

"path": "/healthz",

"port": 10251

},

"initialDelaySeconds": 15,

"timeoutSeconds": 15

}

}

\]

}

}

通常，不需要额外设置scheduler。



你或许想加载 /var/log 并将输出记录在这个日志目录里。



Controller Manager Template

完成controller manager pod的template：



{

"kind": "Pod",

"apiVersion": "v1",

"metadata": {

"name": "kube-controller-manager"

},"spec": {

"hostNetwork": true,

"containers": \[

{

"name": "kube-controller-manager",

"image": "$HYPERKUBE\_IMAGE",

"command": \[

"/hyperkube",

"controller-manager",

"$CNTRLMNGR\_FLAG1",

...

"$CNTRLMNGR\_FLAGN"

\],

"volumeMounts": \[

{

"name": "srvkube",

"mountPath": "/srv/kubernetes",

"readOnly": true

},

{

"name": "etcssl",

"mountPath": "/etc/ssl",

"readOnly": true

}

\],

"livenessProbe": {

"httpGet": {

"host": "127.0.0.1",

"path": "/healthz",

"port": 10252

},

"initialDelaySeconds": 15,

"timeoutSeconds": 15

}

}

\],

"volumes": \[

{

"name": "srvkube",

"hostPath": {

"path": "/srv/kubernetes"

}

},

{

"name": "etcssl",

"hostPath": {

"path": "/etc/ssl"

}

}

\]

}

}

配合controller manager所使用的选项：



–cluster-name=$CLUSTER\_NAME

—cluster-cidr=

TODO: 解释这个选项

–allocate-node-cidrs=

TODO: 解释这个选项

–cloud-provider= 和 –cloud-config 在apiserver章节里解释过。

–service-account-private-key-file=/srv/kubernetes/server.key ，这个值是service

account功能所使用的。

–master=127.0.0.1:8080

 运行和验证Apiserver，Scheduler和Controller Manager

将每个完成的pod template放置在kubelet的配置文件夹中（文件夹地址是在kubelet的 –config= 选项所指向的地址， 通常是 /etc/kubernetes/manifests ）。没有放置顺序关系:scheduler和controller manager会一直尝试连接到apiserver，直到连接成功。



用 ps 或者 docker ps 来检测每一个进程是否正常运行。 比如，你可以这样看apiserver的容器是否被kubelet启动了：



$ sudo docker ps \| grep apiserver:

5783290746d5 gcr.io/google\_containers/kube-apiserver:e36bf367342b5a80d7467fd7611ad873........

之后尝试连接apiserver:



$ echo $\(curl -s http://localhost:8080/healthz\)

ok

$ curl -s http://localhost:8080/api

{

"versions": \[

"v1"

\]

}

如果kubelets使用 –register-node=true 这个选项，他们会开始自动注册到apiserver上。很快，你就可以使用 kubectl get nodes 命令看到所有的节点。否则，你需要手动注册这些节点。



日志

TODO 如何开启日志。



监控

TODO 如何开启监控。



DNS

TODO 如何运行DNS。



故障排除

运行validate-cluster

TODO 解释如何运行“cluster/validate-cluster.sh”。



检查pods和services

你可以尝试这阅读“检查的集群”这一节，例如GCE。你应该检查Service。通过“mirro pods”去检查apiserver，scehduler和controller-manager以及运行的插件。



例子

到这里你应该能够运行一些基本的实例了，例如nginx example。



运行测试

你可以试着运行一致性测试. 测试失败的结果可能会给你些排除故障的线索。



网络

节点之间必须用私有IP链接。可以通过ping或者SSH来确定节点之间的联通。

