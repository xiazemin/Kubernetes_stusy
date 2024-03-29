下载最新的 Docker for Mac 或者 Edge 版本，即可以看到内置的 Kubernetes 集群，直接点击安装即可在本地搭建好单节点的 Kubernetes 环境：













安装完毕后，如果我们也勾选了 Show system containers 选项，那么使用如下的 Docker 命令，能看到自动安装的 Kubernetes 相关容器：





```
➜  ~ docker container ls --format "table{{.Names}}\t{{.Image }}\t{{.Command}}"
```



NAMES                                                                                                                   IMAGE                                                    COMMAND

k8s\_compose\_compose-75f8bb4779-stxv9\_docker\_3c963862-f9f4-11e7-93cc-025000000001\_0                                      docker/kube-compose-controller                           "/compose-controller…"

k8s\_POD\_compose-75f8bb4779-stxv9\_docker\_3c963862-f9f4-11e7-93cc-025000000001\_0                                          gcr.io/google\_containers/pause-amd64:3.0                 "/pause"

k8s\_sidecar\_kube-dns-545bc4bfd4-799pr\_kube-system\_139bf000-f9f4-11e7-93cc-025000000001\_0                                gcr.io/google\_containers/k8s-dns-sidecar-amd64           "/sidecar --v=2 --lo…"

k8s\_dnsmasq\_kube-dns-545bc4bfd4-799pr\_kube-system\_139bf000-f9f4-11e7-93cc-025000000001\_0                                gcr.io/google\_containers/k8s-dns-dnsmasq-nanny-amd64     "/dnsmasq-nanny -v=2…"

k8s\_kubedns\_kube-dns-545bc4bfd4-799pr\_kube-system\_139bf000-f9f4-11e7-93cc-025000000001\_0                                gcr.io/google\_containers/k8s-dns-kube-dns-amd64          "/kube-dns --domain=…"

k8s\_kube-proxy\_kube-proxy-rrd8t\_kube-system\_139b00df-f9f4-11e7-93cc-025000000001\_0                                      gcr.io/google\_containers/kube-proxy-amd64                "/usr/local/bin/kube…"

k8s\_POD\_kube-dns-545bc4bfd4-799pr\_kube-system\_139bf000-f9f4-11e7-93cc-025000000001\_0                                    gcr.io/google\_containers/pause-amd64:3.0                 "/pause"

k8s\_POD\_kube-proxy-rrd8t\_kube-system\_139b00df-f9f4-11e7-93cc-025000000001\_0                                             gcr.io/google\_containers/pause-amd64:3.0                 "/pause"

k8s\_kube-scheduler\_kube-scheduler-docker-for-desktop\_kube-system\_972d74c9fc2f4ebd8ab673058e386a65\_0                     gcr.io/google\_containers/kube-scheduler-amd64            "kube-scheduler --ad…"

k8s\_kube-apiserver\_kube-apiserver-docker-for-desktop\_kube-system\_f7a81e8fe624bd46059fc6084e86bb81\_0                     gcr.io/google\_containers/kube-apiserver-amd64            "kube-apiserver --ad…"

k8s\_etcd\_etcd-docker-for-desktop\_kube-system\_56a21c0a5f545c0cca5388c457bb1b3b\_0                                         gcr.io/google\_containers/etcd-amd64                      "etcd --advertise-cl…"

k8s\_kube-controller-manager\_kube-controller-manager-docker-for-desktop\_kube-system\_8d1848c1e562e35a225e402988eadcd1\_0   gcr.io/google\_containers/kube-controller-manager-amd64   "kube-controller-man…"

k8s\_POD\_kube-apiserver-docker-for-desktop\_kube-system\_f7a81e8fe624bd46059fc6084e86bb81\_0                                gcr.io/google\_containers/pause-amd64:3.0                 "/pause"

k8s\_POD\_kube-controller-manager-docker-for-desktop\_kube-system\_8d1848c1e562e35a225e402988eadcd1\_0                       gcr.io/google\_containers/pause-amd64:3.0                 "/pause"

k8s\_POD\_kube-scheduler-docker-for-desktop\_kube-system\_972d74c9fc2f4ebd8ab673058e386a65\_0                                gcr.io/google\_containers/pause-amd64:3.0                 "/pause"

k8s\_POD\_etcd-docker-for-desktop\_kube-system\_56a21c0a5f545c0cca5388c457bb1b3b\_0                                          gcr.io/google\_containers/pause-amd64:3.0                 "/pause"

关于各个容器的作用，可以参阅 这里。在安装过程中，Docker 也为我们安装了 kubectl 控制命令：



$ kubectl get namespaces

$ kubectl get posts --namespace kube-system

接下来我们可以使用 kubectl 命令来创建简单的 kubernetes-dashboard 服务：



➜  ~ kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml

secret "kubernetes-dashboard-certs" created

serviceaccount "kubernetes-dashboard" created

role "kubernetes-dashboard-minimal" created

rolebinding "kubernetes-dashboard-minimal" created

deployment "kubernetes-dashboard" created

service "kubernetes-dashboard" created

服务安装完毕后可以查看部署的容器与服务：



➜  ~ kubectl get deployments --namespace kube-system

NAME                   DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE

kube-dns               1         1         1            1           22m

kubernetes-dashboard   1         1         1            0           26s

➜  ~ kubectl get services --namespace kube-system

NAME                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT\(S\)         AGE

kube-dns               ClusterIP   10.96.0.10      &lt;none&gt;        53/UDP,53/TCP   22m

kubernetes-dashboard   ClusterIP   10.111.242.95   &lt;none&gt;        443/TCP         30s

在 Dashboard 启动完毕后，可以使用 kubectl 提供的 Proxy 服务来访问该面板：



$ kubectl proxy



\# 打开如下地址：

\# http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/

如果访问报错，可以尝试编辑 kubernetes-dashboard 服务，或者参阅这里：



$ kubectl -n kube-system edit service kubernetes-dashboard



\# Please edit the object below. Lines beginning with a '\#' will be ignored,

\# and an empty file will abort the edit. If an error occurs while saving this file will be

\# reopened with the relevant failures.

\#

apiVersion: v1

...

  name: kubernetes-dashboard

  namespace: kube-system

  resourceVersion: "343478"

  selfLink: /api/v1/namespaces/kube-system/services/kubernetes-dashboard-head

  uid: 8e48f478-993d-11e7-87e0-901b0e532516

spec:

  clusterIP: 10.100.124.90

  externalTrafficPolicy: Cluster

  ports:

  - port: 443

    protocol: TCP

    targetPort: 8443

  selector:

    k8s-app: kubernetes-dashboard

  sessionAffinity: None

  type: ClusterIP -&gt;&gt; NodePort

status:

  loadBalancer: {}

访问上述地址，我们可以看到登录界面：













此时可暂时直接跳过，进入到控制面板中：























Docker 同样为我们提供了简单的应用示范，可以直接使用如下的 Docker Compose 配置文件:



version: '3.3'



services:

  web:

    build: web

    image: dockerdemos/lab-web

    volumes:

     - "./web/static:/static"

    ports:

     - "80:80"



  words:

    build: words

    image: dockerdemos/lab-words

    deploy:

      replicas: 5

      endpoint\_mode: dnsrr

      resources:

        limits:

          memory: 16M

        reservations:

          memory: 16M



  db:

    build: db

    image: dockerdemos/lab-db

然后使用 stack 命令创建应用栈：



$ docker stack deploy --compose-file stack.yml demo



Stack demo was created

Waiting for the stack to be stable and running...

 - Service web has one container running

� 应用栈创建完毕后，可以使用 kubectl 查看创建的 Pods:



$ kubectl get pods



NAME                     READY     STATUS    RESTARTS   AGE

db-7f99cc64b7-cbd9t      1/1       Running   0          2m

web-758c6998f8-tmxfm     1/1       Running   0          2m

words-54bf6c5d57-8bxc8   1/1       Running   0          2m

words-54bf6c5d57-dzxm8   1/1       Running   0          2m

words-54bf6c5d57-k2448   1/1       Running   0          2m

words-54bf6c5d57-mhh4p   1/1       Running   0          2m

words-54bf6c5d57-w2q82   1/1       Running   0          2m

也可以来查看部署的集群与服务：



$ kubectl get deployments

NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE

db        1         1         1            1           3m

web       1         1         1            1           3m

words     5         5         5            5           3m



$ kubectl get services

NAME         TYPE           CLUSTER-IP     EXTERNAL-IP   PORT\(S\)        AGE

db           ClusterIP      None           &lt;none&gt;        55555/TCP      3m

kubernetes   ClusterIP      10.96.0.1      &lt;none&gt;        443/TCP        52m

web          LoadBalancer   10.97.154.28   &lt;pending&gt;     80:30577/TCP   3m

words        ClusterIP      None           &lt;none&gt;        55555/TCP      3m

可以看到这里的 web 有所谓的 LoadBalancer 类型，即可以对外提供服务。最后我们还可以用 stack 与 kubectl 命令来删除应用：



$ docker stack remove demo

$ kubectl delete deployment kubernetes-dashboard --namespace kube-system

