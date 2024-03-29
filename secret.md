Secret解决了密码、token、密钥等敏感数据的配置问题，而不需要把这些敏感数据暴露到镜像或者Pod Spec中。Secret可以以Volume或者环境变量的方式使用。



Secret有三种类型：



Service Account：用来访问Kubernetes API，由Kubernetes自动创建，并且会自动挂载到Pod的/run/secrets/kubernetes.io/serviceaccount目录中；

Opaque：base64编码格式的Secret，用来存储密码、密钥等；

kubernetes.io/dockerconfigjson：用来存储私有docker registry的认证信息。

Opaque Secret

Opaque类型的数据是一个map类型，要求value是base64编码格式：



$ echo -n "admin" \| base64

YWRtaW4=

$ echo -n "1f2d1e2e67df" \| base64

MWYyZDFlMmU2N2Rm

secrets.yml



apiVersion: v1

kind: Secret

metadata:

  name: mysecret

type: Opaque

data:

  password: MWYyZDFlMmU2N2Rm

  username: YWRtaW4=

接着，就可以创建secret了：kubectl create -f secrets.yml。



创建好secret之后，有两种方式来使用它：



以Volume方式

以环境变量方式

将Secret挂载到Volume中

apiVersion: v1

kind: Pod

metadata:

  labels:

    name: db

  name: db

spec:

  volumes:

  - name: secrets

    secret:

      secretName: mysecret

  containers:

  - image: gcr.io/my\_project\_id/pg:v1

    name: db

    volumeMounts:

    - name: secrets

      mountPath: "/etc/secrets"

      readOnly: true

    ports:

    - name: cp

      containerPort: 5432

      hostPort: 5432

将Secret导出到环境变量中

apiVersion: extensions/v1beta1

kind: Deployment

metadata:

  name: wordpress-deployment

spec:

  replicas: 2

  strategy:

      type: RollingUpdate

  template:

    metadata:

      labels:

        app: wordpress

        visualize: "true"

    spec:

      containers:

      - name: "wordpress"

        image: "wordpress"

        ports:

        - containerPort: 80

        env:

        - name: WORDPRESS\_DB\_USER

          valueFrom:

            secretKeyRef:

              name: mysecret

              key: username

        - name: WORDPRESS\_DB\_PASSWORD

          valueFrom:

            secretKeyRef:

              name: mysecret

              key: password

kubernetes.io/dockerconfigjson

可以直接用kubectl命令来创建用于docker registry认证的secret：



$ kubectl create secret docker-registry myregistrykey --docker-server=DOCKER\_REGISTRY\_SERVER --docker-username=DOCKER\_USER --docker-password=DOCKER\_PASSWORD --docker-email=DOCKER\_EMAIL

secret "myregistrykey" created.

也可以直接读取~/.docker/config.json的内容来创建：



$ cat ~/.docker/config.json \| base64

$ cat &gt; myregistrykey.yaml &lt;&lt;EOF

apiVersion: v1

kind: Secret

metadata:

  name: myregistrykey

data:

  .dockerconfigjson: UmVhbGx5IHJlYWxseSByZWVlZWVlZWVlZWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGxsbGx5eXl5eXl5eXl5eXl5eXl5eXl5eSBsbGxsbGxsbGxsbGxsbG9vb29vb29vb29vb29vb29vb29vb29vb29vb25ubm5ubm5ubm5ubm5ubm5ubm5ubm5ubmdnZ2dnZ2dnZ2dnZ2dnZ2dnZ2cgYXV0aCBrZXlzCg==

type: kubernetes.io/dockerconfigjson

EOF

$ kubectl create -f myregistrykey.yaml

在创建Pod的时候，通过imagePullSecrets来引用刚创建的myregistrykey:



apiVersion: v1

kind: Pod

metadata:

  name: foo

spec:

  containers:

    - name: foo

      image: janedoe/awesomeapp:v1

  imagePullSecrets:

    - name: myregistrykey

Service Account

Service Account用来访问Kubernetes API，由Kubernetes自动创建，并且会自动挂载到Pod的/run/secrets/kubernetes.io/serviceaccount目录中。



$ kubectl run nginx --image nginx

deployment "nginx" created

$ kubectl get pods

NAME                     READY     STATUS    RESTARTS   AGE

nginx-3137573019-md1u2   1/1       Running   0          13s

$ kubectl exec nginx-3137573019-md1u2 ls /run/secrets/kubernetes.io/serviceaccount

ca.crt

namespace

token

