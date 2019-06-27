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



