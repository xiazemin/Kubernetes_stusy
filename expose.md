```
$kubectl expose deployment goapp-deploy --type=LoadBalancer --name=goapp-expose --namespace=kube-apps
service "goapp-expose" exposed
```

访问成功

```
$kubectl get services --namespace=kube-apps
NAME           TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
goapp-expose   LoadBalancer   10.105.237.133   localhost     8085:31267/TCP   2m
goapp-svc      ClusterIP      10.100.253.241   <none>        8085/TCP         15h
```



