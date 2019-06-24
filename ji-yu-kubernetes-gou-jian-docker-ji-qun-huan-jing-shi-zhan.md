kubernetes是google公司基于docker所做的一个分布式集群，有以下主件组成

etcd

: 高可用存储共享配置和服务发现，作为与minion机器上的flannel配套使用，作用是使每台 minion上运行的docker拥有不同的ip段，最终目的是使不同minion上正在运行的docker containner都有一个与别的任意一个containner（别的minion上运行的docker containner）不一样的IP地址。

flannel

: 网络结构支持

kube-apiserver: 

不论通过kubectl还是使用remote api 直接控制，都要经过apiserver

　　kube-controller-manager

: 对replication controller, endpoints controller, namespace controller, and serviceaccounts controller的循环控制，与kube-apiserver交互，保证这些controller工作

　kube-scheduler

: Kubernetes scheduler的作用就是根据特定的调度算法将pod调度到指定的工作节点（minion）上，这一过程也叫绑定（bind\)

kubelet

: Kubelet运行在Kubernetes Minion Node上. 它是container agent的逻辑继任者

kube-proxy

: kube-proxy是kubernetes 里运行在minion节点上的一个组件, 它起的作用是一个服务代理的角色

图为GIT+Jenkins+Kubernetes+Docker+Etcd+confd+Nginx+Glusterfs架构

：

如下：

![](http://images2015.cnblogs.com/blog/1087716/201704/1087716-20170401135152164-1257503094.png)



环境：

centos7系统机器三台：

    10.0.0.81: 用来安装kubernetes master

    10.0.0.82: 用作kubernetes minion （minion1）

    10.0.0.83: 用作kubbernetes minion \(minion2\)

一、关闭系统运行的防火墙及selinux

1。如果系统开启了防火墙则按如下步骤关闭防火墙（所有机器）

\# systemctl stop firewalld \# systemctl disable firewalld

2.关闭selinux

```
#setenforce 0
#sed -i '/^SELINUX=/cSELINUX=disabled' /etc/sysconfig/selinux
```

二、MASTER安装配置

1. 

安装并配置Kubernetes master\(yum 方式\)

| `# yum -y install etcd kubernetes` |
| :--- |




 配置etcd。确保列出的这些项都配置正确并且没有被注释掉，下面的配置都是如此　

| 123456 | `#vim /etc/etcd/etcd.confETCD_NAME=defaultETCD_DATA_DIR="/var/lib/etcd/default.etcd"ETCD_LISTEN_CLIENT_URLS="http://0.0.0.0:2379"ETCD_ADVERTISE_CLIENT_URLS="http://localhost:2379"` |
| :--- | :--- |


配置kubernetes

| 12345678 | `vim /etc/kubernetes/apiserverKUBE_API_ADDRESS="--address=0.0.0.0"KUBE_API_PORT="--port=8080"KUBELET_PORT="--kubelet_port=10250"KUBE_ETCD_SERVERS="--etcd_servers=http://127.0.0.1:2379"KUBE_SERVICE_ADDRESSES="--service-cluster-ip-range=10.254.0.0/16"KUBE_ADMISSION_CONTROL="--admission_control=NamespaceLifecycle,NamespaceExists,LimitRanger,SecurityContextDeny,ResourceQuota"KUBE_API_ARGS=""` |
| :--- | :--- |




2. 启动etcd

, kube-apiserver, kube-controller-manager and kube-scheduler服务

| 1 | `# for SERVICES in etcd kube-apiserver kube-controller-manager kube-scheduler; do systemctl restart $SERVICES systemctl enable $SERVICES systemctl status $SERVICES done` |
| :--- | :--- |


3.设置etcd网络

| 1 | `#etcdctl -C 10.0.0.81:2379 set /atomic.io/network/config '{"Network":"10.1.0.0/16"}'` |
| :--- | :--- |


4. 至此master配置完成，运行kubectl get nodes可以查看有多少minion在运行，以及其状态。这里我们的minion还都没有开始安装配置，所以运行之后结果为空

| 1 | `# kubectl get nodes NAME LABELS STATUS` |
| :--- | :--- |




三、MINION安装配置\(每台minion机器都按如下安装配置\)

1. 环境安装和配置

| 1 | `# yum -y install flannel kubernetes` |
| :--- | :--- |


　　配置kubernetes连接的服务端IP

| 123 | `#vim /etc/kubernetes/configKUBE_MASTER="--master=http://10.0.0.81:8080"KUBE_ETCD_SERVERS="--etcd_servers=http://10.0.0.81:2379"` |
| :--- | :--- |


配置kubernetes  ,（请使用每台minion自己的IP地址比如10.0.0.81：代替下面的$LOCALIP）

| 12345 | `#vim /etc/kubernetes/kubelet<br>KUBELET_ADDRESS="--address=0.0.0.0"KUBELET_PORT="--port=10250"# change the hostname to this host’s IP address KUBELET_HOSTNAME="--hostname_override=$LOCALIP"KUBELET_API_SERVER="--api_servers=http://10.0.0.81:8080"KUBELET_ARGS=""` |
| :--- | :--- |




2. 准备启动服务

（如果本来机器上已经运行过docker的请看过来，没有运行过的请忽略此步骤）

    运行ifconfig，查看机器的网络配置情况（有docker0）

| 12345 | `# ifconfig docker0Link encap:Ethernet HWaddr 02:42:B2:75:2E:67 inet addr:172.17.0.1 Bcast:0.0.0.0 Mask:255.255.0.0 UPBROADCAST MULTICAST MTU:1500 Metric:1 RX packets:0 errors:0 dropped:0 overruns:0 frame:0 TX packets:0errors:0 dropped:0 overruns:0 carrier:0 collisions:0 txqueuelen:0RX bytes:0 (0.0 B) TX bytes:0 (0.0 B)` |
| :--- | :--- |


　 warning:在运行过docker的机器上可以看到有docker0，这里在启动服务之前需要删掉docker0配置，在命令行运行:sudo ip link delete docker0

3.配置flannel网络

| 123 | `#vim /etc/sysconfig/flanneldFLANNEL_ETCD_ENDPOINTS="http://10.0.0.81:2379"FLANNEL_ETCD_PREFIX="/atomic.io/network"` |
| :--- | :--- |


PS：其中atomic.io与上面etcd中的Network对应

4. 启动服务

| 1 | `# for SERVICES in flanneld kube-proxy kubelet docker; do systemctl restart $SERVICES systemctl enable $SERVICES systemctl status $SERVICES done` |
| :--- | :--- |




四、配置完成验证安装

    确定两台minion\(10.0.0.82和10.0.0.83\)和一台master（10.0.0.81）都已经成功的安装配置并且服务都已经启动了。

    切换到master机器上，运行命令kubectl get nodes 

| 1234 | `# kubectl get nodesNAME STATUS AGE10.0.0.82 Ready 1m10.0.0.83 Ready 1m` |
| :--- | :--- |


　　可以看到配置的两台minion已经在master的node列表中了。如果想要更多的node，只需要按照minion的配置，配置更多的机器就可以了。

