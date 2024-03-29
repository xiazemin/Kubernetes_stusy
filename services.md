Kubernetes Pod是平凡的，它门会被创建，也会死掉（生老病死），并且他们是不可复活的。 ReplicationControllers动态的创建和销毁Pods\(比如规模扩大或者缩小，或者执行动态更新\)。每个pod都由自己的ip，这些IP也随着时间的变化也不能持续依赖。这样就引发了一个问题：如果一些Pods（让我们叫它作后台，后端）提供了一些功能供其它的Pod使用（让我们叫作前台），在kubernete集群中是如何实现让这些前台能够持续的追踪到这些后台的？



答案是：Service



Kubernete Service 是一个定义了一组Pod的策略的抽象，我们也有时候叫做宏观服务。这些被服务标记的Pod都是（一般）通过label Selector决定的（下面我们会讲到我们为什么需要一个没有label selector的服务）



举个例子，我们假设后台是一个图形处理的后台，并且由3个副本。这些副本是可以相互替代的，并且前台并需要关心使用的哪一个后台Pod，当这个承载前台请求的pod发生变化时，前台并不需要直到这些变化，或者追踪后台的这些副本，服务是这些去耦



对于Kubernete原生的应用，Kubernete提供了一个简单的Endpoints API，这个Endpoints api的作用就是当一个服务中的pod发生变化时，Endpoints API随之变化，对于哪些不是原生的程序，Kubernetes提供了一个基于虚拟IP的网桥的服务，这个服务会将请求转发到对应的后台pod



Defining a service\(定义一个服务\)

一个Kubernete服务是一个最小的对象，类似pod,和其它的终端对象一样，我们可以朝paiserver发送请求来创建一个新的实例，比如，假设你拥有一些Pod,每个pod都开放了9376端口，并且均带有一个标签app=MyApp



{



“kind”: “Service”,



“apiVersion”: “v1”,



“metadata”: {



“name”: “my-service”



},



“spec”: {



“selector”: {



“app”: “MyApp”



},



“ports”: \[



{



“protocol”: “TCP”,



“port”: 80,



“targetPort”: 9376



}



\]



}



}



这段代码会创建一个新的服务对象，名称为：my-service，并且会连接目标端口9376，并且带有label app=MyApp的pod,这个服务会被分配一个ip地址，这个ip是给服务代理使用的（下面我们会看到），服务的选择器会持续的评估，并且结果会被发送到一个Endpoints 对象，这个Endpoints的对象的名字也叫“my-service”.



服务可以将一个“入端口”转发到任何“目标端口”，默认情况下targetPort的值会和port的值相同，更有趣的是，targetPort可以是字符串，可以指定到一个name,这个name是pod的一个端口。并且实际指派给这个name的端口可以是不同在不同的后台pod中，这样让我们能更加灵活的部署我们的服务，比如；我们可以在下一个更新版本中修改后台pod暴露的端口而不会影响客户的使用（更新过程不会打断）



服务支持tcp和UDP，但是默认的是TCP

Services without selectors（没有选择器的服务）

服务总体上抽象了对Pod的访问，但是服务也抽象了其它的内容，比如：



1：比如你希望有一个额外的数据库云在生产环境中，但是在测试的时候，我们希望使用自己的数据库



2：我们希望将服务指向其它的服务或者其它命名空间或者其它的云平台上的服务



3：我们正在向kubernete迁移，并且我们后台并没有在Kubernete中



如上的情况下，我们可以定义一个服务没有选择器



{



“kind”: “Service”,



“apiVersion”: “v1″,



“metadata”: {



“name”: “my-service”



},



“spec”: {



“ports”: \[



{



“protocol”: “TCP”,



“port”: 80,



“targetPort”: 9376



}



\]



}



}



因为没有选择器，所以相应的Endpoints对象就不会被创建，但是我们手动把我们的服务和Endpoints对应起来



{



“kind”: “Endpoints”,



“apiVersion”: “v1″,



“metadata”: {



“name”: “my-service”



},



“subsets”: \[



{



“addresses”: \[



{ “IP”: “1.2.3.4” }



\],



“ports”: \[



{ “port”: 80 }



\]



}



\]



}



这样的话，这个服务虽然没有selector，但是却可以正常工作，所有的请求都会被转发到1.2.3.4：80



Virtual IPs and service proxies（虚拟IP和服务代理）

每一个节点上都运行了一个kube-proxy，这个应用监控着Kubermaster增加和删除服务，对于每一个服务，kube-proxy会随机开启一个本机端口，任何发向这个端口的请求都会被转发到一个后台的Pod当中，而如何选择是哪一个后台的pod的是基于SessionAffinity进行的分配。kube-proxy会增加iptables rules来实现捕捉这个服务的Ip和端口来并重定向到前面提到的端口。



最终的结果就是所有的对于这个服务的请求都会被转发到后台的Pod中，这一过程用户根本察觉不到



20161014161523



默认的，后台的选择是随机的，基于用户session机制的策略可以通过修改service.spec.sessionAffinity 的值从NONE到ClientIP



Multi-Port Services（多端口服务）

可能很多服务需要开发不止一个端口,为了满足这样的情况，Kubernetes允许在定义时候指定多个端口，当我们使用多个端口的时候，我们需要指定所有端口的名称，这样endpoints才能清楚，例如



{



“kind”: “Service”,



“apiVersion”: “v1”,



“metadata”: {



“name”: “my-service”



},



“spec”: {



“selector”: {



“app”: “MyApp”



},



“ports”: \[



{



“name”: “http”,



“protocol”: “TCP”,



“port”: 80,



“targetPort”: 9376



},



{



“name”: “https”,



“protocol”: “TCP”,



“port”: 443,



“targetPort”: 9377



}



\]



}



}



选择自己的IP地址

我们可以在创建服务的时候指定IP地址，将spec.clusterIP的值设定为我们想要的IP地址即可。例如，我们已经有一个DNS域我们希望用来替换，或者遗留系统只能对指定IP提供服务，并且这些都非常难修改，用户选择的IP地址必须是一个有效的IP地址，并且要在API server分配的IP范围内，如果这个IP地址是不可用的，apiserver会返回422http错误代码来告知是IP地址不可用



为什么不使用循环的DNS

一个问题持续的被提出来，这个问题就是我们为什么不使用标准的循环DNS而使用虚拟IP，我们主要有如下几个原因



1：DNS不遵循TTL查询和缓存name查询的问题由来已久（这个还真不知道，就是DNS更新的问题估计）



2：许多的应用的DNS查询查询一次后就缓存起来



3：即使如上亮点被解决了，但是不停的进行DNS进行查询，大量的请求也是很难被管理的



我们希望阻止用户使用这些可能会“伤害”他们的事情，但是如果足够多的人要求这么作，那么我们将对此提供支持，来作为一个可选项.



Discovering services（服务的发现）

Kubernetes 支持两种方式的来发现服务 ，环境变量和 DNS



环境变量

当一个Pod在一个node上运行时，kubelet 会针对运行的服务增加一系列的环境变量，它支持Docker links compatible 和普通环境变量



举例子来说：



redis-master服务暴露了 TCP 6379端口，并且被分配了10.0.0.11 IP地址



那么我们就会有如下的环境变量



REDIS\_MASTER\_SERVICE\_HOST=10.0.0.11



REDIS\_MASTER\_SERVICE\_PORT=6379



REDIS\_MASTER\_PORT=tcp://10.0.0.11:6379



REDIS\_MASTER\_PORT\_6379\_TCP=tcp://10.0.0.11:6379



REDIS\_MASTER\_PORT\_6379\_TCP\_PROTO=tcp



REDIS\_MASTER\_PORT\_6379\_TCP\_PORT=6379



REDIS\_MASTER\_PORT\_6379\_TCP\_ADDR=10.0.0.11



这样的话，对系统有一个要求：所有的想要被POD访问的服务，必须在POD创建之前创建，否则这个环境变量不会被填充，使用DNS则没有这个问题



DNS

一个可选择的云平台插件就是DNS，DNS 服务器监控着API SERVER ，当有服务被创建的时候，DNS 服务器会为之创建相应的记录，如果DNS这个服务被添加了，那么Pod应该是可以自动解析服务的。



举个例子来说：如果我们在my-ns命名空间下有一个服务叫做“my-service”，这个时候DNS就会创建一个my-service.my-ns的记录，所有my-ns命名空间下的Pod,都可以通过域名my-service查询找到对应的ip地址，不同命名空间下的Pod在查找是必须使用my-sesrvice.my-ns才可以。



Kubernete 同样支持端口的解析，如果my-service有一个提供http的TCP主持的端口，那么我们可以通过查询“\_http.\_tcp.my-service.my-ns”来查询这个端口



Headless services

有时候我们可能不需要一个固定的IP和分发，这个时候我们只需要将spec.clusterIP的值设置为none就可以了



对于这样的服务来说，集群IP没有分配，这个时候当你查询服务的名称的时候，DNS会返回多个A记录，这些记录都是指向后端Pod的。Kube 代理不会处理这个服务，在服务的前端也没有负载均衡器。但是endpoints controller还是会创建Endpoints



（好吧，这个好处貌似我还理解好）



This option allows developers to reduce coupling to the Kubernetes system, if they desire, but leaves them freedom to do discovery in their own way. Applications can still use a self-registration pattern and adapters for other discovery systems could easily be built upon this API.



External services（外部服务）

对于我们的应用程序来说，我们可能有一部分是放在Kubernete外部的（比如我们有单独的物理机来承担数据库的角色），Kubernetes支持两种方式：NodePorts，LoadBalancers



每一个服务都会有一个字段定义了该服务如何被调用（发现），这个字段的值可以为：



ClusterIP:使用一个集群固定IP，这个是默认选项

NodePort：使用一个集群固定IP，但是额外在每个POD上均暴露这个服务，端口

LoadBalancer：使用集群固定IP，和NODEPord,额外还会申请申请一个负载均衡器来转发到服务（load balancer ）

注意：NodePort 支持TCP和UDN，但是LoadBalancers在1.0版本只支持TCP



Type NodePort

如果你选择了“NodePort”，那么 Kubernetes master 会分配一个区域范围内，（默认是30000-32767），并且，每一个node，都会代理（proxy）这个端口到你的服务中，我们可以在spec.ports\[\*\].nodePort 找到具体的值



如果我们向指定一个端口，我们可以直接写在nodePort上，系统就会给你指派指定端口，但是这个值必须是指定范围内的。



这样的话就能够让开发者搭配自己的负载均衡器，去撘建一个kubernete不是完全支持的系统，又或者是直接暴露一个node的ip地址



Type LoadBalancer

在支持额外的负载均衡器的的平台上，将值设置为LoadBalancer会提供一个负载均衡器给你的服务，负载均衡器的创建其实是异步的。下面的例子



{



“kind”: “Service”,



“apiVersion”: “v1″,



“metadata”: {



“name”: “my-service”



},



“spec”: {



“selector”: {



“app”: “MyApp”



},



“ports”: \[



{



“protocol”: “TCP”,



“port”: 80,



“targetPort”: 9376,



“nodePort”: 30061



}



\],



“clusterIP”: “10.0.171.239”,



“type”: “LoadBalancer”



},



“status”: {



“loadBalancer”: {



“ingress”: \[



{



“ip”: “146.148.47.155”



}



\]



}



}



}



所有服务的请求均会直接到到Pod,具体是如何工作的是由平台决定的



缺点



我们希望使用IPTABLES和用户命名空间来代理虚拟IP能在中小型规模的平台上正常运行，但是可能出现问题在比较大的平台上当应对成千上万的服务的时候。



这个时候，使用kube-proxy来封装服务的请求，这使得这些变成可能



LoadBalancers 只支持TCP，不支持UDP



Type 的值是设定好的，不同的值代表不同的功能，并不是所有的平台都需要的，但是是所有API需要的



Future work

在将来，我们预想proxy的策略能够更加细致，不再是单纯的转发，比如master-elected or sharded，我们预想将来服务会拥有真正的负载均衡器，到时候虚拟IP直接转发到负载均衡器



将来有倾向与将所的工作均通过iptables来进行，从而小从用户命名空间代理，这样的话会有更好的性能和消除一些原地值IP的问题，尽管这样的会减少一些灵活性.

