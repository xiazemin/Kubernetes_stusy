[https://github.com/kubernetes/dashboard](https://github.com/kubernetes/dashboard)

```
15:25:12-didi@bogon:~$docker run -d -p 8088:8088 --name webserver nginx
fb0f1fc58755a993ef7383195f67312bc882df2b6aa827cb8806b121c5495493
15:25:20-didi@bogon:~$curl http://127.0.0.1:8088
curl: (52) Empty reply from server
15:25:35-didi@bogon:~$docker stop webserver
webserver
15:25:51-didi@bogon:~$curl http://127.0.0.1:8088
curl: (7) Failed to connect to 127.0.0.1 port 8088: Connection refused
15:25:52-didi@bogon:~$kubectl get nodes
NAME                 STATUS    ROLES     AGE       VERSION
docker-for-desktop   Ready     master    3h        v1.10.3
15:26:06-didi@bogon:~$kubectl cluster-info
Kubernetes master is running at https://localhost:6443
KubeDNS is running at https://localhost:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
15:26:18-didi@bogon:~$ kubectl get namespaces
NAME          STATUS    AGE
default       Active    3h
docker        Active    3h
kube-public   Active    3h
kube-system   Active    3h
15:28:21-didi@bogon:~$ kubectl get posts --namespace kube-system
error: the server doesn't have a resource type "posts"
15:28:28-didi@bogon:~$kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
secret "kubernetes-dashboard-certs" created
serviceaccount "kubernetes-dashboard" created
role.rbac.authorization.k8s.io "kubernetes-dashboard-minimal" created
rolebinding.rbac.authorization.k8s.io "kubernetes-dashboard-minimal" created
deployment.apps "kubernetes-dashboard" created
service "kubernetes-dashboard" created
15:29:42-didi@bogon:~$ kubectl get deployments --namespace kube-system
apiVersion: v1
kind: Service
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"Service","metadata":{"annotations":{},"labels":{"k8s-app":"kubernetes-dashboard"},"name":"kubernetes-dashboard","namespace":"kube-system"},"spec":{"ports":[{"port":443,"targetPort":8443}],"selector":{"k8s-app":"kubernetes-dashboard"}}}
  creationTimestamp: 2019-06-26T07:29:42Z
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kube-system
  resourceVersion: "9922"
NAME                   DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
kube-dns               1         1         1            1           3h
kubernetes-dashboard   1         1         1            0           32s
15:30:13-didi@bogon:~$kubectl get services --namespace kube-system
NAME                   TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)         AGE
kube-dns               ClusterIP   10.96.0.10       <none>        53/UDP,53/TCP   3h
kubernetes-dashboard   ClusterIP   10.105.148.225   <none>        443/TCP         43s
15:30:25-didi@bogon:~$kubectl proxy
Starting to serve on 127.0.0.1:8001
```



