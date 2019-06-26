StatefulSet是为了解决有状态服务的问题（对应Deployments和ReplicaSets是为无状态服务而设计），其应用场景包括



稳定的持久化存储，即Pod重新调度后还是能访问到相同的持久化数据，基于PVC来实现

稳定的网络标志，即Pod重新调度后其PodName和HostName不变，基于Headless Service（即没有Cluster IP的Service）来实现

有序部署，有序扩展，即Pod是有顺序的，在部署或者扩展的时候要依据定义的顺序依次依次进行（即从0到N-1，在下一个Pod运行之前所有之前的Pod必须都是Running和Ready状态），基于init containers来实现

有序收缩，有序删除（即从N-1到0）

从上面的应用场景可以发现，StatefulSet由以下几个部分组成：



用于定义网络标志（DNS domain）的Headless Service

用于创建PersistentVolumes的volumeClaimTemplates

定义具体应用的StatefulSet

StatefulSet中每个Pod的DNS格式为statefulSetName-{0..N-1}.serviceName.namespace.svc.cluster.local，其中



serviceName为Headless Service的名字

0..N-1为Pod所在的序号，从0开始到N-1

statefulSetName为StatefulSet的名字

namespace为服务所在的namespace，Headless Servic和StatefulSet必须在相同的namespace

.cluster.local为Cluster Domain，

简单示例

以一个简单的nginx服务web.yaml为例：



---

apiVersion: v1

kind: Service

metadata:

  name: nginx

  labels:

    app: nginx

spec:

  ports:

  - port: 80

    name: web

  clusterIP: None

  selector:

    app: nginx

---

apiVersion: apps/v1beta1

kind: StatefulSet

metadata:

  name: web

spec:

  serviceName: "nginx"

  replicas: 2

  template:

    metadata:

      labels:

        app: nginx

    spec:

      containers:

      - name: nginx

        image: gcr.io/google\_containers/nginx-slim:0.8

        ports:

        - containerPort: 80

          name: web

        volumeMounts:

        - name: www

          mountPath: /usr/share/nginx/html

  volumeClaimTemplates:

  - metadata:

      name: www

      annotations:

        volume.alpha.kubernetes.io/storage-class: anything

    spec:

      accessModes: \[ "ReadWriteOnce" \]

      resources:

        requests:

          storage: 1Gi

$ kubectl create -f web.yaml

service "nginx" created

statefulset "web" created



\# 查看创建的headless service和statefulset

$ kubectl get service nginx

NAME      CLUSTER-IP   EXTERNAL-IP   PORT\(S\)   AGE

nginx     None         &lt;none&gt;        80/TCP    1m

$ kubectl get statefulset web

NAME      DESIRED   CURRENT   AGE

web       2         2         2m



\# 根据volumeClaimTemplates自动创建PVC（在GCE中会自动创建kubernetes.io/gce-pd类型的volume）

$ kubectl get pvc

NAME        STATUS    VOLUME                                     CAPACITY   ACCESSMODES   AGE

www-web-0   Bound     pvc-d064a004-d8d4-11e6-b521-42010a800002   1Gi        RWO           16s

www-web-1   Bound     pvc-d06a3946-d8d4-11e6-b521-42010a800002   1Gi        RWO           16s



\# 查看创建的Pod，他们都是有序的

$ kubectl get pods -l app=nginx

NAME      READY     STATUS    RESTARTS   AGE

web-0     1/1       Running   0          5m

web-1     1/1       Running   0          4m



\# 使用nslookup查看这些Pod的DNS

$ kubectl run -i --tty --image busybox dns-test --restart=Never --rm /bin/sh

/ \# nslookup web-0.nginx

Server:    10.0.0.10

Address 1: 10.0.0.10 kube-dns.kube-system.svc.cluster.local



Name:      web-0.nginx

Address 1: 10.244.2.10

/ \# nslookup web-1.nginx

Server:    10.0.0.10

Address 1: 10.0.0.10 kube-dns.kube-system.svc.cluster.local



Name:      web-1.nginx

Address 1: 10.244.3.12

/ \# nslookup web-0.nginx.default.svc.cluster.local

Server:    10.0.0.10

Address 1: 10.0.0.10 kube-dns.kube-system.svc.cluster.local



Name:      web-0.nginx.default.svc.cluster.local

Address 1: 10.244.2.10

还可以进行其他的操作



\# 扩容

$ kubectl scale statefulset web --replicas=5



\# 缩容

$ kubectl patch statefulset web -p '{"spec":{"replicas":3}}'



\# 镜像更新（目前还不支持直接更新image，需要patch来间接实现）

$ kubectl patch statefulset web --type='json' -p='\[{"op": "replace", "path": "/spec/template/spec/containers/0/image", "value":"gcr.io/google\_containers/nginx-slim:0.7"}\]'



\# 删除StatefulSet和Headless Service

$ kubectl delete statefulset web

$ kubectl delete service nginx



\# StatefulSet删除后PVC还会保留着，数据不再使用的话也需要删除

$ kubectl delete pvc www-web-0 www-web-1

zookeeper

另外一个更能说明StatefulSet强大功能的示例为zookeeper.yaml。



---

apiVersion: v1

kind: Service

metadata:

  name: zk-headless

  labels:

    app: zk-headless

spec:

  ports:

  - port: 2888

    name: server

  - port: 3888

    name: leader-election

  clusterIP: None

  selector:

    app: zk

---

apiVersion: v1

kind: ConfigMap

metadata:

  name: zk-config

data:

  ensemble: "zk-0;zk-1;zk-2"

  jvm.heap: "2G"

  tick: "2000"

  init: "10"

  sync: "5"

  client.cnxns: "60"

  snap.retain: "3"

  purge.interval: "1"

---

apiVersion: policy/v1beta1

kind: PodDisruptionBudget

metadata:

  name: zk-budget

spec:

  selector:

    matchLabels:

      app: zk

  minAvailable: 2

---

apiVersion: apps/v1beta1

kind: StatefulSet

metadata:

  name: zk

spec:

  serviceName: zk-headless

  replicas: 3

  template:

    metadata:

      labels:

        app: zk

      annotations:

        pod.alpha.kubernetes.io/initialized: "true"

        scheduler.alpha.kubernetes.io/affinity: &gt;

            {

              "podAntiAffinity": {

                "requiredDuringSchedulingRequiredDuringExecution": \[{

                  "labelSelector": {

                    "matchExpressions": \[{

                      "key": "app",

                      "operator": "In",

                      "values": \["zk-headless"\]

                    }\]

                  },

                  "topologyKey": "kubernetes.io/hostname"

                }\]

              }

            }

    spec:

      containers:

      - name: k8szk

        imagePullPolicy: Always

        image: gcr.io/google\_samples/k8szk:v1

        resources:

          requests:

            memory: "4Gi"

            cpu: "1"

        ports:

        - containerPort: 2181

          name: client

        - containerPort: 2888

          name: server

        - containerPort: 3888

          name: leader-election

        env:

        - name : ZK\_ENSEMBLE

          valueFrom:

            configMapKeyRef:

              name: zk-config

              key: ensemble

        - name : ZK\_HEAP\_SIZE

          valueFrom:

            configMapKeyRef:

                name: zk-config

                key: jvm.heap

        - name : ZK\_TICK\_TIME

          valueFrom:

            configMapKeyRef:

                name: zk-config

                key: tick

        - name : ZK\_INIT\_LIMIT

          valueFrom:

            configMapKeyRef:

                name: zk-config

                key: init

        - name : ZK\_SYNC\_LIMIT

          valueFrom:

            configMapKeyRef:

                name: zk-config

                key: tick

        - name : ZK\_MAX\_CLIENT\_CNXNS

          valueFrom:

            configMapKeyRef:

                name: zk-config

                key: client.cnxns

        - name: ZK\_SNAP\_RETAIN\_COUNT

          valueFrom:

            configMapKeyRef:

                name: zk-config

                key: snap.retain

        - name: ZK\_PURGE\_INTERVAL

          valueFrom:

            configMapKeyRef:

                name: zk-config

                key: purge.interval

        - name: ZK\_CLIENT\_PORT

          value: "2181"

        - name: ZK\_SERVER\_PORT

          value: "2888"

        - name: ZK\_ELECTION\_PORT

          value: "3888"

        command:

        - sh

        - -c

        - zkGenConfig.sh && zkServer.sh start-foreground

        readinessProbe:

          exec:

            command:

            - "zkOk.sh"

          initialDelaySeconds: 15

          timeoutSeconds: 5

        livenessProbe:

          exec:

            command:

            - "zkOk.sh"

          initialDelaySeconds: 15

          timeoutSeconds: 5

        volumeMounts:

        - name: datadir

          mountPath: /var/lib/zookeeper

      securityContext:

        runAsUser: 1000

        fsGroup: 1000

  volumeClaimTemplates:

  - metadata:

      name: datadir

      annotations:

        volume.alpha.kubernetes.io/storage-class: anything

    spec:

      accessModes: \[ "ReadWriteOnce" \]

      resources:

        requests:

          storage: 20Gi

kubectl create -f zookeeper.yaml

详细的使用说明见zookeeper stateful application。



StatefulSet注意事项

还在beta状态，需要kubernetes v1.5版本以上才支持

所有Pod的Volume必须使用PersistentVolume或者是管理员事先创建好

为了保证数据安全，删除StatefulSet时不会删除Volume

StatefulSet需要一个Headless Service来定义DNS domain，需要在StatefulSet之前创建好

目前StatefulSet还没有feature complete，比如更新操作还需要手动patch。

