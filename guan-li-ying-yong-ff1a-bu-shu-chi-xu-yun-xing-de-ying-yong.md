在前面的章节里，我们了解了如何用 kubectl run 快速部署一个简单的复制的应用以及如何用pods（configuring-containers.md）配置并生成单次运行的容器。本文，我们将使用基于配置的方法来部署一个持续运行的复制的应用。



用配置文件生成复制品集合

Kubernetes用 Replication Controllers 创建并管理复制的容器集合（实际上是复制的Pods）。 Replication Controller 简单地确保在任一时间里都有特定数量的pod副本在运行。如果运行的太多，它会杀掉一些；如果运行的太少，它会启动一些。这和谷歌计算引擎的Instance Group Manager以及AWS的Auto-scaling Group（不带扩展策略）类似。在快速开始章节里用 kubctl run 创建的用来跑Nginx的 Replication Controller 可以用下面的YAML描述：



apiVersion: v1

kind: ReplicationController

metadata:

name: my-nginx

spec:

replicas: 2

template:

metadata:

labels:

app: nginx

spec:

containers:

- name: nginx

image: nginx

ports:

- containerPort: 80

和指定一个单独的Pod相比，不同的是设置了这里的kind域为ReplicationController，设定了需要的副本（replicas）数量以及把Pod的定义放到了template域下面。pods的名字不需要显示指定，因为它们是由 replication controller 的名字生成的。要查看支持的域列表，可以看replication controller API object。 和创建pods一样，也可以用 create 命令来创建这个replication controller：



$ kubectl create -f ./nginx-rc.yaml

replicationcontrollers/my-nginx

replication controller 会替换删除的或者因不明原因终止的（比如节点失败）pods，这和直接创建的pods的情况是不一样。基于这样的考量，对于一个需要持续运行的应用，即便你的应用只需要一个单独的pod，我们也推荐使用 replication controller 。对于单独的pod，在配置文件里可以省略 replicas 这个域，因为不设置的时候默认就只有一个副本。



查看replication controller的状态

可以用 get 命令查看你创建的replication controller：



$ kubectl get rc

CONTROLLER  CONTAINER\(S\)   IMAGE\(S\)   SELECTOR    REPLICAS

my-nginx        nginx                  nginx      app=nginx  2

这说明你的controller会确保有两个nginx的副本。和直接创建的pod一样，也可以用 get 命令查看这些副本：



$ kubectl get pods

NAME             READY  STATUS    RESTARTS  AGE

my-nginx-065jq   1/1    Running   0         51s

my-nginx-buaiq   1/1    Running   0         51s

删除replication controllers

如果想要结束你的应用并且删除repication controller。和在快速开始里一样，用下面的命令：



$ kubectl delete rc my-nginx

replicationcontrollers/my-nginx

这个操作默认会把由replication controller管理的pods一起删除。如果pods的数量比较大，这个操作要花一些时间才能完成。如果想要pods继续运行，不被删掉，可以在delete的时候指定参数 –cascade=false 。 如果在删除replication controller之前想要删除pods，pods只是被替换了，因为replication controller会再起新的pods，确保pods的数量。



Labels

Kubernetes使用自定义的键值对（称为Labels）分类资源集合，例如pods和replicationcontroller。在前面的例子里，pod的模板里只设定了一个单独的label，键是 app ，值为 nginx 。所有被创建的pod都带有这个label，可以用带-L参数的命令查看：



$ kubectl get pods -L app

NAME            READY  STATUS   RESTARTS  AGE  APP

my-nginx-afv12  0/1    Running  0         3s   nginx

my-nginx-lg99z  0/1    Running  0         3s   nginx

pod模板带的label默认会被复制为replication controller的label。Kubernetes中所有的资源都支持labels：



$ kubectl get rc my-nginx -L app

CONTROLLER  CONTAINER\(S\)  IMAGE\(S\)  SELECTOR   REPLICAS  APP

my-nginx    nginx         nginx     app=nginx  2         nginx

更重要的是，pod模板的label会被用来创建 selector ，这个 selector 会匹配所有带这些labels的pods。用 kubectl get 的go语言模板输出格式就可以看到这个域：



$ kubectl get rc my-nginx -o template --template="{{.spec.selector}}"

map\[app:nginx\]

如果你想要在pod模板里指定labels，但是又不想要被选中，可以显示指定 selector 来解决，不过需要确保 selector 能够匹配由pod模板创建出来的pod的label，并且不能匹配由其他replication controller创建的pods。对于后者，最直接最保险的方法是给replication controller分配一个独特的label，并且在pod模板和selector里都进行指定。

