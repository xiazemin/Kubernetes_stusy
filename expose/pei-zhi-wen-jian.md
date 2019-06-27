

发送反馈

Kubernetes Engine   文档

使用 Service 公开应用

本页面介绍如何在 Google Kubernetes Engine 集群中创建 Kubernetes Service。有关 Service 概念的说明以及各种 Service 类型的讨论，请参阅 Service。



简介

Service 的理念是将一组 Pod 端点划分为单一资源。您可以配置各种方式来访问该分组。默认情况下，您会获得稳定的集群 IP 地址，集群内部的客户端可以使用该 IP 地址与 Service 中的 Pod 通信。客户端向稳定 IP 地址发送请求，然后请求会被路由到 Service 的其中一个 Pod。



提供五种类型的 Service：



ClusterIP（默认）

NodePort

LoadBalancer

ExternalName

Headless

本主题提供多个练习。在每个练习中，您将创建 Deployment 并将通过创建 Service 来公开其 Pod。然后，您将向 Service 发送 HTTP 请求。



准备工作

请执行以下步骤为此任务做准备：



确保您已启用 Google Kubernetes Engine API。

启用 GOOGLE KUBERNETES ENGINE API

确保您已安装 Cloud SDK。

设置默认项目 ID：

gcloud config set project \[PROJECT\_ID\]

如果您使用的是地区级集群，请设置默认计算地区：

gcloud config set compute/zone \[COMPUTE\_ZONE\]

如果您使用的是区域级集群，请设置默认计算区域：

gcloud config set compute/region \[COMPUTE\_REGION\]

将 gcloud 更新到最新版本：

gcloud components update

注意：您可以使用 --project、--zone 和 --region 操作标志在 gcloud 命令中替换这些默认设置。

创建 ClusterIP 类型的 Service

KUBECTL APPLYCONSOLE

以下是 Deployment 的清单：



apiVersion: apps/v1

kind: Deployment

metadata:

  name: my-deployment

spec:

  selector:

    matchLabels:

      app: metrics

      department: sales

  replicas: 3

  template:

    metadata:

      labels:

        app: metrics

        department: sales

    spec:

      containers:

      - name: hello

        image: "gcr.io/google-samples/hello-app:2.0"

将该清单复制到名为 my-deployment.yaml 的文件，然后创建该 Deployment：



kubectl apply -f my-deployment.yaml

验证三个 Pod 是否正在运行：



kubectl get pods

输出会显示三个正在运行的 Pod：



NAME                              READY     STATUS    RESTARTS   AGE

service-how-to-76699757f9-h4xk4   1/1       Running   0          4s

service-how-to-76699757f9-tjcfq   1/1       Running   0          4s

service-how-to-76699757f9-wt9d8   1/1       Running   0          4s

以下是 ClusterIP 类型的 Service 的清单：



apiVersion: v1

kind: Service

metadata:

  name: my-cip-service

spec:

  type: ClusterIP

  selector:

    app: metrics

    department: sales

  ports:

  - protocol: TCP

    port: 80

    targetPort: 8080

该 Service 所具有的选择器指定了两个标签：



app: metrics

department: sales

您之前创建的 Deployment 中的每个 Pod 都有这两个标签。因此，Deployment 中的 Pod 将成为此 Service 的成员。



将该清单复制到名为 my-cip-service.yaml 的文件，然后创建该 Service：



kubectl apply -f my-cip-service.yaml

等待 Kubernetes 为 Service 分配稳定的内部地址，然后查看 Service：



kubectl get service my-cip-service --output yaml

输出会显示 clusterIP 的值。



spec:

  clusterIP: 10.59.241.241

记下您的 clusterIP 值以备后续使用。



访问 Service

列出正在运行的 Pod：



kubectl get pods

在输出中，复制以 my-deployment 开头的其中一个 Pod 名称。



NAME                               READY     STATUS    RESTARTS   AGE

my-deployment-6897d9577c-7z4fv     1/1       Running   0          5m

获取其中一个正在运行的容器的 shell：



kubectl exec -it \[YOUR\_POD\_NAME\] -- sh

其中 \[YOUR\_POD\_NAME\] 是 my-deployment 的其中一个 Pod 的名称。



在 shell 中，安装 curl：



apk add --no-cache curl

在容器中，通过使用您的集群 IP 地址和端口 80，向 Service 发出请求。请注意，80 是 Service 的 port 字段的值。这是用作 Service 客户端的端口。



curl \[CLUSTER\_IP\]:80

其中 \[CLUSTER\_IP\] 是 Service 中 clusterIP 的值。



您的请求将转发到 TCP 端口 8080 上的其中一个成员 Pod，而 8080 是 targetPort 字段的值。请注意，Service 的每个成员 Pod 必须具有一个侦听端口 8080 的容器。



响应会显示 hello-app 的输出：



Hello, world!

Version: 2.0.0

Hostname: service-how-to-76699757f9-hsb5x

要退出容器的 shell，请输入 exit。



注意：您需要提前了解，每个成员 Pod 都具有一个侦听 TCP 端口 8080 的容器。在本练习中，您未执行任何操作来使容器侦听端口 8080。要确认 hello-app 是否在侦听端口 8080，您可以查看应用的 Dockerfile 和源代码。

创建 NodePort 类型的 Service

KUBECTL APPLYCONSOLE

以下是 Deployment 的清单：



apiVersion: apps/v1

kind: Deployment

metadata:

  name: my-deployment-50000

spec:

  selector:

    matchLabels:

      app: metrics

      department: engineering

  replicas: 3

  template:

    metadata:

      labels:

        app: metrics

        department: engineering

    spec:

      containers:

      - name: hello

        image: "gcr.io/google-samples/hello-app:2.0"

        env:

        - name: "PORT"

          value: "50000"

注意清单中的 env 对象。env 对象指定正在运行的容器的 PORT 环境变量的值将为 50000。hello-app 应用侦听 PORT 环境变量所指定的端口。因此在本练习中，您指示容器侦听端口 50000。



将该清单复制到名为 my-deployment-50000.yaml 的文件，然后创建该 Deployment：



kubectl apply -f my-deployment-50000.yaml

验证三个 Pod 是否正在运行：



kubectl get pods

以下是 NodePort 类型的 Service 的清单：



apiVersion: v1

kind: Service

metadata:

  name: my-np-service

spec:

  type: NodePort

  selector:

    app: metrics

    department: engineering

  ports:

  - protocol: TCP

    port: 80

    targetPort: 50000

将该清单复制到名为 my-np-service.yaml 的文件，然后创建该 Service：



kubectl apply -f my-np-service.yaml

查看 Service：



kubectl get service my-np-service --output yaml

输出会显示 nodePort 值。



...

  spec:

    ...

    ports:

    - nodePort: 30876

      port: 80

      protocol: TCP

      targetPort: 50000

    selector:

      app: metrics

      department: engineering

    sessionAffinity: None

    type: NodePort

...

如果集群中的节点具有外部 IP 地址，请找到其中一个节点的外部 IP 地址：



kubectl get nodes --output wide

输出会显示节点的外部 IP 地址：



NAME          STATUS    ROLES     AGE    VERSION        EXTERNAL-IP

gke-svc-...   Ready     none      1h     v1.9.7-gke.6   203.0.113.1

并非所有集群的节点都有外部 IP 地址。例如，专用集群中的节点没有外部 IP 地址。



创建一条防火墙规则以允许 TCP 流量进入节点端口：



gcloud compute firewall-rules create test-node-port --allow tcp:\[NODE\_PORT\]

其中：\[NODE\_PORT\] 是 Service 的 nodePort 字段的值。



访问 Service

在浏览器的地址栏中输入 \[NODE\_IP\_ADDRESS\]:\[NODE\_PORT\]。



其中：



\[NODE\_IP\_ADDRESS\] 是其中一个节点的外部 IP 地址。

\[NODE\_PORT\] 是您的节点端口值。

响应会显示 hello-app 的输出：



Hello, world!

Version: 2.0.0

Hostname: service-how-to-50000-695955857d-q76pb

创建 LoadBalancer 类型的 Service

KUBECTL APPLYCONSOLE

以下是 Deployment 的清单：



apiVersion: apps/v1

kind: Deployment

metadata:

  name: my-deployment-50001

spec:

  selector:

    matchLabels:

      app: products

      department: sales

  replicas: 3

  template:

    metadata:

      labels:

        app: products

        department: sales

    spec:

      containers:

      - name: hello

        image: "gcr.io/google-samples/hello-app:2.0"

        env:

        - name: "PORT"

          value: "50001"

请注意，此 Deployment 中的容器将侦听端口 50001。



将该清单复制到名为 my-deployment-50001.yaml 的文件，然后创建该 Deployment：



kubectl apply -f my-deployment-50001.yaml

验证三个 Pod 是否正在运行：



kubectl get pods

以下是 LoadBalancer 类型的 Service 的清单：



apiVersion: v1

kind: Service

metadata:

  name: my-lb-service

spec:

  type: LoadBalancer

  selector:

    app: products

    department: sales

  ports:

  - protocol: TCP

    port: 60000

    targetPort: 50001

将该清单复制到名为 my-lb-service.yaml, 的文件，然后创建该 Service：



kubectl apply -f my-lb-service.yaml

创建 LoadBalancer 类型的 Service 时，Google Cloud 控制器会唤醒并配置网络负载平衡器。等待一分钟，让控制器配置网络负载平衡器并生成稳定 IP 地址。



查看 Service：



kubectl get service my-lb-service --output yaml

输出的 loadBalancer:ingress: 下方会显示稳定的外部 IP 地址。



...

spec:

  ...

  ports:

  - ...

    port: 60000

    protocol: TCP

    targetPort: 50001

  selector:

    app: products

    department: sales

  sessionAffinity: None

  type: LoadBalancer

status:

  loadBalancer:

    ingress:

    - ip: 203.0.113.10

访问 Service

等待几分钟，让 GKE 配置负载平衡器。



在浏览器的地址栏中输入 \[LOAD\_BALANCER\_ADDRESS\]:60000。



其中，\[LOAD\_BALANCER\_ADDRESS\] 是您的负载平衡器的外部 IP 地址。



响应会显示 hello-app 的输出：



Hello, world!

Version: 2.0.0

Hostname: service-how-to-50001-644f8857c7-xxdwg

请注意，Service 中 port 的值是任意的。在上述示例中，使用的 port 值为 60000。



创建 ExternalName 类型的 Service

ExternalName 类型的 Service 为外部 DNS 名称提供内部别名。内部客户端使用内部 DNS 名称发出请求，然后请求会被重定向到外部名称。



以下是 ExternalName 类型的 Service 的清单：



apiVersion: v1

kind: Service

metadata:

  name: my-xn-service

spec:

  type: ExternalName

  externalName: example.com

在上述示例中，DNS 名称是 my-xn-service.default.svc.cluster.local。当内部客户端向 my-xn-service.default.svc.cluster.local 发出请求时，请求会被重定向到 example.com。



使用 kubectl expose 创建 Service

作为编写 Service 清单的替代方法，您可以使用 kubectl expose 公开 Deployment，以创建 Service。



要公开本主题前面显示的 my-deployment，您可以输入以下命令：



kubectl expose deployment my-deployment --name my-cip-service \

    --type ClusterIP --protocol TCP --port 80 --target-port 8080

要公开本主题前面显示的 my-deployment-50000，您可以输入以下命令：



kubectl expose deployment my-deployment-50000 --name my-np-service \

    --type NodePort --protocol TCP --port 80 --target-port 50000

要公开本主题前面显示的 my-deployment-50001，您可以输入以下命令：



kubectl expose deployment my-deployment-50001 --name my-lb-service \

    --type LoadBalancer --port 60000 --target-port 50001

清理

完成本页面上的练习后，请按照以下步骤移除资源，防止您的帐号产生不必要的费用：



KUBECTL APPLYCONSOLE

删除 Service

kubectl delete services my-cip-service my-np-service my-lb-service

删除 Deployment

kubectl delete deployments my-deployment my-deployment-50000 my-deployment-50001

删除防火墙规则

gcloud compute firewall-rules delete test-node-port

