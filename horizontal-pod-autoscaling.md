Horizontal Pod Autoscaling可以根据CPU使用率或应用自定义metrics自动扩展Pod数量（支持replication controller、deployment和replica set）。



控制管理器每隔30s（可以通过–horizontal-pod-autoscaler-sync-period修改）查询metrics的资源使用情况

支持三种metrics类型

预定义metrics（比如Pod的CPU）以利用率的方式计算

自定义的Pod metrics，以原始值（raw value）的方式计算

自定义的object metrics

支持两种metrics查询方式：Heapster和自定义的REST API

支持多metrics

示例

\# 创建pod和service

$ kubectl run php-apache --image=gcr.io/google\_containers/hpa-example --requests=cpu=200m --expose --port=80

service "php-apache" created

deployment "php-apache" created



\# 创建autoscaler

$ kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10

deployment "php-apache" autoscaled

$ kubectl get hpa

NAME         REFERENCE                     TARGET    CURRENT   MINPODS   MAXPODS   AGE

php-apache   Deployment/php-apache/scale   50%       0%        1         10        18s



\# 增加负载

$ kubectl run -i --tty load-generator --image=busybox /bin/sh

Hit enter for command prompt

$ while true; do wget -q -O- http://php-apache.default.svc.cluster.local; done



\# 过一会就可以看到负载升高了

$ kubectl get hpa

NAME         REFERENCE                     TARGET    CURRENT   MINPODS   MAXPODS   AGE

php-apache   Deployment/php-apache/scale   50%       305%      1         10        3m



\# autoscaler将这个deployment扩展为7个pod

$ kubectl get deployment php-apache

NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE

php-apache   7         7         7            7           19m



\# 删除刚才创建的负载增加pod后会发现负载降低，并且pod数量也自动降回1个

$ kubectl get hpa

NAME         REFERENCE                     TARGET    CURRENT   MINPODS   MAXPODS   AGE

php-apache   Deployment/php-apache/scale   50%       0%        1         10        11m



$ kubectl get deployment php-apache

NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE

php-apache   1         1         1            1           27m

自定义metrics

使用方法



控制管理器开启–horizontal-pod-autoscaler-use-rest-clients

控制管理器的–apiserver指向API Server Aggregator

在API Server Aggregator中注册自定义的metrics API

注：可以参考k8s.io/metics开发自定义的metrics API server。



比如HorizontalPodAutoscaler保证每个Pod占用50% CPU、1000pps以及10000请求/s：



HPA示例



apiVersion: autoscaling/v2alpha1 kind: HorizontalPodAutoscaler metadata: name: php-apache namespace: default spec: scaleTargetRef: apiVersion: apps/v1beta1 kind: Deployment name: php-apache minReplicas: 1 maxReplicas: 10 metrics: - type: Resource resource: name: cpu targetAverageUtilization: 50 - type: Pods pods: metricName: packets-per-second targetAverageValue: 1k - type: Object object: metricName: requests-per-second target: apiVersion: extensions/v1beta1 kind: Ingress name: main-route targetValue: 10k status: observedGeneration: 1 lastScaleTime: &lt;some-time&gt; currentReplicas: 1 desiredReplicas: 1 currentMetrics: - type: Resource resource: name: cpu currentAverageUtilization: 0 currentAverageValue: 0

参考：https://feisky.gitbooks.io/kubernetes/concepts/autoscaling.html

