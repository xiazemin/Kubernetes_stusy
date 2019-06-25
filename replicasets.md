ReplicaSet是下一代复本控制器。ReplicaSet和 Replication Controller之间的唯一区别是现在的选择器支持。Replication Controller只支持基于等式的selector（env=dev或environment!=qa），但ReplicaSet还支持新的，基于集合的selector（version in \(v1.0, v2.0\)或env notin \(dev, qa\)）。在试用时官方推荐ReplicaSet。



大多数kubectl支持Replication Controller的命令也支持ReplicaSets。rolling-update命令有一个例外 。如果您想要滚动更新功能，请考虑使用Deployments。此外， rolling-update命令是必须的，而Deployments是声明式的，因此我们建议通过rollout命令使用Deployments。



虽然ReplicaSets可以独立使用，但是今天它主要被 Deployments 作为协调pod创建，删除和更新的机制。当您使用Deployments时，您不必担心管理他们创建的ReplicaSets。Deployments拥有并管理其ReplicaSets。



何时使用ReplicaSet？

ReplicaSet可确保指定数量的pod“replicas”在任何设定的时间运行。然而，Deployments是一个更高层次的概念，它管理ReplicaSets，并提供对pod的声明性更新以及许多其他的功能。因此，我们建议您使用Deployments而不是直接使用ReplicaSets，除非您需要自定义更新编排或根本不需要更新。



这实际上意味着您可能永远不需要操作ReplicaSet对象：直接使用Deployments并在规范部分定义应用程序。



例

frontend.yaml 将frontend.yaml复制到剪贴板

apiVersion: extensions/v1beta1

kind: ReplicaSet

metadata:

  name: frontend

  \# these labels can be applied automatically

  \# from the labels in the pod template if not set

  \# labels:

    \# app: guestbook

    \# tier: frontend

spec:

  \# this replicas value is default

  \# modify it according to your case

  replicas: 3

  \# selector can be applied automatically

  \# from the labels in the pod template if not set,

  \# but we are specifying the selector here to

  \# demonstrate its usage.

  selector:

    matchLabels:

      tier: frontend

    matchExpressions:

      - {key: tier, operator: In, values: \[frontend\]}

  template:

    metadata:

      labels:

        app: guestbook

        tier: frontend

    spec:

      containers:

      - name: php-redis

        image: gcr.io/google\_samples/gb-frontend:v3

        resources:

          requests:

            cpu: 100m

            memory: 100Mi

        env:

        - name: GET\_HOSTS\_FROM

          value: dns

          \# If your cluster config does not include a dns service, then to

          \# instead access environment variables to find service host

          \# info, comment out the 'value: dns' line above, and uncomment the

          \# line below.

          \# value: env

        ports:

        - containerPort: 80

将此配置保存frontend.yaml到Kubernetes集群并将其提交给Kubernetes集群时，应创建定义的ReplicaSet及其管理的pod。



$ kubectl create -f frontend.yaml

replicaset "frontend" created

$ kubectl describe rs/frontend

Name:          frontend

Namespace:     default

Image\(s\):      gcr.io/google\_samples/gb-frontend:v3

Selector:      tier=frontend,tier in \(frontend\)

Labels:        app=guestbook,tier=frontend

Replicas:      3 current / 3 desired

Pods Status:   3 Running / 0 Waiting / 0 Succeeded / 0 Failed

No volumes.

Events:

  FirstSeen    LastSeen    Count    From                SubobjectPath    Type        Reason            Message

  ---------    --------    -----    ----                -------------    --------    ------            -------

  1m           1m          1        {replicaset-controller }             Normal      SuccessfulCreate  Created pod: frontend-qhloh

  1m           1m          1        {replicaset-controller }             Normal      SuccessfulCreate  Created pod: frontend-dnjpy

  1m           1m          1        {replicaset-controller }             Normal      SuccessfulCreate  Created pod: frontend-9si5l

$ kubectl get pods

NAME             READY     STATUS    RESTARTS   AGE

frontend-9si5l   1/1       Running   0          1m

frontend-dnjpy   1/1       Running   0          1m

frontend-qhloh   1/1       Running   0          1m

