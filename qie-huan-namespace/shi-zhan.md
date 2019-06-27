$kubectl get namespaces --show-labels

NAME          STATUS    AGE       LABELS

default       Active    23h       &lt;none&gt;

docker        Active    23h       &lt;none&gt;

kube-apps     Active    14h       &lt;none&gt;

kube-public   Active    23h       &lt;none&gt;

kube-system   Active    23h       &lt;none&gt;

$        kubectl get deployment --namespace=kube-apps

NAME           DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE

goapp-deploy   2         2         2            2           14h

$        kubectl get pods --namespace=kube-apps

NAME                            READY     STATUS    RESTARTS   AGE

goapp-deploy-767986448b-8t5d8   1/1       Running   0          13h

goapp-deploy-767986448b-dfn5d   1/1       Running   0          13h

$ kubectl proxy --namespace=kube-apps

F0627 11:00:36.200633   38414 proxy.go:154\] listen tcp 127.0.0.1:8001: bind: address already in use

$ kubectl proxy --namespace=kube-apps

Starting to serve on 127.0.0.1:8001

