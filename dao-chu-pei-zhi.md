[http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/\#!/login](http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/login)

[http://127.0.0.1:8085/ping](http://127.0.0.1:8085/ping)

PONG

kubectl get deployment

NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE

db           1         1         1            1           16h

goxiazemin   1         1         1            1           13h

my-nginx     2         2         2            2           22h

web          1         1         1            0           16h

words        5         5         5            1           16h

$kubectl get deployment/goxiazemin -o=yaml --export &gt; goxiazemin-deploy.yaml

```
$cat goxiazemin-deploy.yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  creationTimestamp: null
  generation: 1
  labels:
    run: goxiazemin
  name: goxiazemin
  selfLink: /apis/extensions/v1beta1/namespaces/default/deployments/goxiazemin
spec:
  progressDeadlineSeconds: 600
  replicas: 1
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      run: goxiazemin
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: goxiazemin
    spec:
      containers:
      - image: xiazemin/docker-multi-stage-demo:latest
        imagePullPolicy: Never
        name: goxiazemin
        ports:
        - containerPort: 8085
          protocol: TCP
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status: {}
```

$kubectl get services/goapp-expose --namespace=kube-apps  -o=yaml --export &gt; goapp-expose.yaml

```
$cat goapp-expose.yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    k8s-app: goappk8s
  name: goapp-expose
  selfLink: /api/v1/namespaces/kube-apps/services/goapp-expose
spec:
  externalTrafficPolicy: Cluster
  ports:
  - nodePort: 31267
    port: 8085
    protocol: TCP
    targetPort: 8085
  selector:
    k8s-app: goappk8s
  sessionAffinity: None
  type: LoadBalancer
status:
  loadBalancer: {}
```

原配置文件加上：type: LoadBalancer

```
$kubectl delete services/goapp-expose --namespace=kube-apps
service "goapp-expose" deleted
 
$kubectl apply -f k8s.yaml

$kubectl get services --namespace=kube-apps
NAME        TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
goapp-svc   LoadBalancer   10.100.253.241   localhost     8085:32408/TCP   15h
```

请求成功



