PodPreset用来给指定标签的Pod注入额外的信息，如环境变量、存储卷等。这样，Pod模板就不需要为每个Pod都显式设置重复的信息。



开启PodPreset

开启API settings.k8s.io/v1alpha1/podpreset

开启准入控制 PodPreset

示例

增加环境变量和存储卷的PodPreset



kind: PodPreset

apiVersion: settings.k8s.io/v1alpha1

metadata:

  name: allow-database

  namespace: myns

spec:

  selector:

    matchLabels:

      role: frontend

  env:

    - name: DB\_PORT

      value: "6379"

  volumeMounts:

    - mountPath: /cache

      name: cache-volume

  volumes:

    - name: cache-volume

      emptyDir: {}

用户提交Pod



apiVersion: v1

kind: Pod

metadata:

  name: website

  labels:

    app: website

    role: frontend

spec:

  containers:

    - name: website

      image: ecorp/website

      ports:

        - containerPort: 80

经过准入控制PodPreset后，Pod会自动增加环境变量和存储卷



apiVersion: v1

kind: Pod

metadata:

  name: website

  labels:

    app: website

    role: frontend

  annotations:

    podpreset.admission.kubernetes.io/allow-database: "resource version"

spec:

  containers:

    - name: website

      image: ecorp/website

      volumeMounts:

        - mountPath: /cache

          name: cache-volume

      ports:

        - containerPort: 80

      env:

        - name: DB\_PORT

          value: "6379"

  volumes:

    - name: cache-volume

      emptyDir: {}

ConfigMap示例

ConfigMap



apiVersion: v1

kind: ConfigMap

metadata:

  name: etcd-env-config

data:

  number\_of\_members: "1"

  initial\_cluster\_state: new

  initial\_cluster\_token: DUMMY\_ETCD\_INITIAL\_CLUSTER\_TOKEN

  discovery\_token: DUMMY\_ETCD\_DISCOVERY\_TOKEN

  discovery\_url: http://etcd\_discovery:2379

  etcdctl\_peers: http://etcd:2379

  duplicate\_key: FROM\_CONFIG\_MAP

  REPLACE\_ME: "a value"

PodPreset



kind: PodPreset

apiVersion: settings.k8s.io/v1alpha1

metadata:

  name: allow-database

  namespace: myns

spec:

  selector:

    matchLabels:

      role: frontend

  env:

    - name: DB\_PORT

      value: 6379

    - name: duplicate\_key

      value: FROM\_ENV

    - name: expansion

      value: $\(REPLACE\_ME\)

  envFrom:

    - configMapRef:

        name: etcd-env-config

  volumeMounts:

    - mountPath: /cache

      name: cache-volume

    - mountPath: /etc/app/config.json

      readOnly: true

      name: secret-volume

  volumes:

    - name: cache-volume

      emptyDir: {}

    - name: secret-volume

      secretName: config-details

用户提交的Pod



apiVersion: v1

kind: Pod

metadata:

  name: website

  labels:

    app: website

    role: frontend

spec:

  containers:

    - name: website

      image: ecorp/website

      ports:

        - containerPort: 80

经过准入控制 PodPreset后，Pod会自动增加ConfigMap环境变量



apiVersion: v1

kind: Pod

metadata:

  name: website

  labels:

    app: website

    role: frontend

  annotations:

    podpreset.admission.kubernetes.io/allow-database: "resource version"

spec:

  containers:

    - name: website

      image: ecorp/website

      volumeMounts:

        - mountPath: /cache

          name: cache-volume

        - mountPath: /etc/app/config.json

          readOnly: true

          name: secret-volume

      ports:

        - containerPort: 80

      env:

        - name: DB\_PORT

          value: "6379"

        - name: duplicate\_key

          value: FROM\_ENV

        - name: expansion

          value: $\(REPLACE\_ME\)

      envFrom:

        - configMapRef:

          name: etcd-env-config

  volumes:

    - name: cache-volume

      emptyDir: {}

    - name: secret-volume

      secretName: config-details

参考：https://feisky.gitbooks.io/kubernetes/concepts/podpreset.html

