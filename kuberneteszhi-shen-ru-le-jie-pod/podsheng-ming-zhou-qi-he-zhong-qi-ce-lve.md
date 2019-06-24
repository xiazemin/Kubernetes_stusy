Pod在整个生命周期过程中被定义为各种状态，熟悉Pod的各种状态有助于理解如何设置Pod的调度策略、重启策略

　　Pod的状态包含以下几种，如图：

![](http://images2015.cnblogs.com/blog/1087716/201704/1087716-20170414114025955-844774077.png)

　　Pod的重启策略（RestartPolicy）应用于Pod内所有的容器，并且仅在Pod所处的Node上由kubelet进行判断和重启操作。当某哥容器异常退出或者健康检查石柏师，kubelet将根据RestartPolicy的设置进行相应的操作

　　Pod的重启策略包括Always、OnFailure及Nerver，默认值为Always。

　　kubelet重启失效容器的时间间隔以sync-frequency乘以2n来计算，例如1、2、4、8倍等，最长延时5分钟，并且成功重启后的10分钟后重置该事件。

　　Pod的重启策略和控制方式息息相关，当前可用于管理Pod的控制器宝库ReplicationController、Job、DaemonSet及直接通过kubelet管理（静态Pod），每种控制器对Pod的重启策略要求如下：

* * RC和DaemonSet：必须设置为Always，需要保证该容器持续运行
  * Job：OnFailure或Nerver，确保容器执行完成后不再重启
  * kubelet：在Pod失效时重启他，不论RestartPolicy设置什么值，并且也不会对Pod进行健康检查

7、Pod健康检查

　　对Pod的健康检查可以通过两类探针来检查：LivenessProbe和ReadinessProbe

* * LivenessProbe探针：用于判断容器是否存活（running状态），如果LivenessProbe探针探测到容器不健康，则kubelet杀掉该容器，并根据容器的重启策略做响应处理
  * ReadinessProbe探针：用于判断容器是否启动完成（ready状态），可以接受请求。如果ReadinessProbe探针探测失败，则Pod的状态被修改。Endpoint Controller将从service的Endpoint中删除包含该容器所在的Pod的Endpoint。

　　kubelet定制执行LivenessProbe探针来诊断容器的健康状况。LivenessProbe有三种事项方式。

（1）ExecAction：在容器内部执行一个命令，如果该命令的返回值为0，则表示容器健康

例：

| 123456789101112131415161718192021 | `apiVersion:v1kind: Podmetadata:name: liveness-execlabel:name: livenessspec:containers:- name: tomcatimage: grc.io/google_containers/tomcatargs:-/bin/sh- -c-echook >/tmp.health;sleep10; rm-fr /tmp/health;sleep600livenessProbe:exec:command:-cat-/tmp/healthinitianDelaySeconds:15timeoutSeconds:1　` |
| :--- | :--- |


（2）TCPSocketAction：通过容器ip地址和端口号执行TCP检查，如果能够建立tcp连接表明容器健康

例：

| 123456789101112 | `kind: Podmetadata:name: pod-with-healthcheckspec:containers:- name: nginximage: nginxlivenessProbe:tcpSocket:port: 80initianDelaySeconds:30timeoutSeconds:1` |
| :--- | :--- |


（3）HTTPGetAction：通过容器Ip地址、端口号及路径调用http get方法，如果响应的状态吗大于200且小于400，则认为容器健康

例：

| 1234567891011121314 | `apiVersion:v1kind: Podmetadata:name: pod-with-healthcheckspec:containers:- name: nginximage: nginxlivenessProbe:httpGet:path:/_status/healthzport: 80initianDelaySeconds:30timeoutSeconds:1` |
| :--- | :--- |




对于每种探针方式，都需要设置initialDelaySeconds和timeoutSeconds两个参数，它们含义如下：

* initialDelaySeconds：启动容器后首次监控检查的等待时间，单位秒
* timeouSeconds：健康检查发送请求后等待响应的超时时间，单位秒。当发生超时就被认为容器无法提供服务无，该容器将被重启



