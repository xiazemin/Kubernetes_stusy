参考

[https://devblogs.microsoft.com/premier-developer/bypassing-authentication-for-the-local-kubernetes-cluster-dashboard/](https://devblogs.microsoft.com/premier-developer/bypassing-authentication-for-the-local-kubernetes-cluster-dashboard/)

[https://github.com/kubernetes/dashboard/wiki/Access-control](https://github.com/kubernetes/dashboard/wiki/Access-control)

[https://github.com/kubernetes/dashboard](https://github.com/kubernetes/dashboard)

```
curl https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml -O
vi kubernetes-dashboard.yaml +116
        args:
          - --enable-skip-login
          - --auto-generate-certificates
kubectl apply -f kubernetes-dashboard.yaml
kubectl proxy
```

[http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/\#!/login](http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/login)

