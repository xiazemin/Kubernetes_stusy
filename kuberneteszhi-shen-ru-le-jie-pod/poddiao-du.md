在Kubernetes系统中，Pod在大部分场景下都只是容器的载体而已，通常需要通过RC、Deployment、DaemonSet、Job等对象来完成Pod的调度和自动控制功能。

　　8.1 RC、Deployment：全自动调度

　　RC的主要功能之一就是自动部署容器应用的多份副本，以及持续监控副本的数量，在集群内始终维护用户指定的副本数量。

在调度策略上，除了使用系统内置的调度算法选择合适的Node进行调度，也可以在Pod的定义中使用NodeSelector或NodeAffinity来指定满足条件的Node进行调度。

1）NodeSelector:定向调度

　　Kubernetes Master上的scheduler服务（kube-Scheduler进程）负责实现Pod的调度，整个过程通过一系列复杂的算法，最终为每个Pod计算出一个最佳的目标节点，通常我们无法知道Pod最终会被调度到哪个节点上。实际情况中，我们需要将Pod调度到我们指定的节点上，可以通过Node的标签和pod的nodeSelector属性相匹配来达到目的。

　　（1）首先通过kubectl label命令给目标Node打上标签

kubectl label nodes 

&lt;

node-name

&gt;

&lt;

label-key

&gt;

=

&lt;

label-value

&gt;

例：

| 1 | `#kubectllabel nodes k8s-node-1 zonenorth` |
| :--- | :--- |


　　（2）然后在Pod定义中加上nodeSelector的设置

例：

| 12345678910111213141516171819202122 | `apiVersion:v1kind: Podmetadata:name: redis-masterlabel:name: redis-masterspec:replicas: 1selector:name: redis-mastertemplate:metadata:labels:name: redis-masterspec:containers:- name: redis-masterimages: kubeguide/redis-masterports:- containerPort: 6379nodeSelector:zone: north　` |
| :--- | :--- |


运行kubectl create -f命令创建Pod，scheduler就会将该Pod调度到拥有zone=north标签的Node上。 如果多个Node拥有该标签，则会根据调度算法在该组Node上选一个可用的进行Pod调度。

需要注意的是：如果集群中没有拥有该标签的Node，则这个Pod也无法被成功调度。

　　2）NodeAffinity：亲和性调度

该调度策略是将来替换NodeSelector的新一代调度策略。由于NodeSelector通过Node的Label进行精确匹配，所有NodeAffinity增加了In、NotIn、Exists、DoesNotexist、Gt、Lt等操作符来选择Node。调度侧露更加灵活。

　　8.2 DaemonSet：特定场景调度

DaemonSet用于管理集群中每个Node上仅运行一份Pod的副本实例，如图

![](http://images2015.cnblogs.com/blog/1087716/201704/1087716-20170414114047517-677453441.png)



这种用法适合一些有下列需求的应用：

* 在每个Node上运行个以GlusterFS存储或者ceph存储的daemon进程
* 在每个Node上运行一个日志采集程序，例如fluentd或者logstach
* 在每个Node上运行一个健康程序，采集Node的性能数据。

DaemonSet的Pod调度策略类似于RC，除了使用系统内置的算法在每台Node上进行调度，也可以在Pod的定义中使用NodeSelector或NodeAffinity来指定满足条件的Node范围来进行调度。

　　8.3 批处理调度

9.Pod的扩容和缩荣

　　在实际生产环境中，我们经常遇到某个服务需要扩容的场景，也有可能因为资源精确需要缩减资源而需要减少服务实例数量，此时我们可以Kubernetes中RC提供scale机制来完成这些工作。

以redis-slave RC为例，已定义的最初副本数量为2，通过kubectl scale命令可以将Pod副本数量重新调整

| 1234567 | `#kubectl scale rc redis-slave --replicas=3ReplicationController"redis-slave"scaled#kubectl get podsNAME READY STATUS RESTARTS AGEredis-slave-1sf23 1/1Running 0 1hredis-slave-54wfk 1/1Running 0 1hredis-slave-3da5y 1/1Running 0 1h　` |
| :--- | :--- |


　　除了可以手工通过kubectl scale命令完成Pod的扩容和缩容操作以外，新版本新增加了Horizontal Podautoscaler\(HPA\)的控制器，用于实现基于CPU使用路进行启动Pod扩容缩容的功能。该控制器基于Mastger的kube-controller-manager服务启动参数 --horizontal-pod-autoscler-sync-period定义的时长（默认30秒），周期性监控目标Pod的Cpu使用率并在满足条件时对ReplicationController或Deployment中的Pod副本数量进行调整，以符合用户定义的平均Pod Cpu使用率，Pod Cpu使用率来源于heapster组件，所以需预先安装好heapster。

10.Pod的滚动升级

　　当集群中的某个服务需要升级时，我们需要停止目前与该服务相关的所有Pod，然后重新拉取镜像并启动。如果集群规模较大，因服务全部停止后升级的方式将导致长时间的服务不可用。由此，Kubernetes提供了rolling-update（滚动升级）功能来解决该问题。

滚动升级通过执行kubectl rolling-update命令一键完成，该命令创建一个新的RC，然后自动控制旧版本的Pod数量逐渐减少到0，同时新的RC中的Pod副本数量从0逐步增加到目标值，最终实现Pod的升级。需要注意的是，系统要求新的RC需要与旧的RC在相同的Namespace内，即不能把别人的资产转到到自家名下。

　　例：将redis-master从1.0版本升级到2.0

| 12345678910111213141516171819202122 | `apiVersion: v1kind: replicationControllermetadata:name: redis-master-v2labels:name: redis-masterVersion: v2spec:replicas: 1selector:name: redis-masterVersion: v2template:labels:name: redis-masterVersion: v2spec:containers:- name: masterimages: kubeguide/redis-master:2.0ports:- containerPort: 6379` |
| :--- | :--- |


　　需要注意的点：

　　（1）RC的name不能与旧的RC名字相同

　　（2）在sele中应至少有一个label与旧的RC的label不同，以标识为新的RC。本例中新增了一个名为version的label与旧的RC区分

　　运行kubectl rolling-update来完成Pod的滚动升级：

| 1 | `#kubectl rolling-update redis-master -f redis-master-controller-v2.yaml ` |
| :--- | :--- |


　　另一种方法就是不使用配置文件，直接用kubectl rolling-update加上--image参数指定新版镜像名来完成Pod的滚动升级

| 1 | `#kubectl rolling-update redis-master --image=redis-master:2.0` |
| :--- | :--- |


　　与使用配置文件的方式不同的是，执行的结果是旧的RC被删除，新的RC仍然使用就的RC的名字。

　　如果在更新过程总发现配置有误，则用户可以中断更新操作，并通过执行kubectl rolling-update-rollback完成Pod版本的回滚。



  


