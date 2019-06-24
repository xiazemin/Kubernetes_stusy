应用部署的一个最佳实践是将应用所需的配置信息于程序进行分离，这样可以使得应用程序被更好的复用，通过不用配置文件也能实现更灵活的功能。将应用打包为容器镜像后，可以通过环境变量或外挂文件的方式在创建容器时进行配置注入。ConfigMap是Kubernetes v1.2版本开始提供的一种统一集群配置管理方案。

　5.1 ConfigMap：容器应用的配置管理

　　容器使用ConfigMap的典型用法如下：

　　（1）生产为容器的环境变量。

　　（2）设置容器启动命令的启动参数（需设置为环境变量）。

　　（3）以Volume的形式挂载为容器内部的文件或目录。

　　ConfigMap以一个或多个key:value的形式保存在Kubernetes系统中共应用使用，既可以用于表示一个变量的值，也可以表示一个完整的配置文件内容。

通过yuaml配置文件或者直接使用kubelet create configmap 命令的方式来创建ConfigMap

　5.2 ConfigMap的创建

　　　举个小例子cm-appvars.yaml来描述将几个应用所需的变量定义为ConfigMap的用法：

  


| 12345678 | `# vim cm-appvars.yamlapiVersion: v1kind: ConfigMapmetadata:name: cm-appvarsdata:apploglevel: infoappdatadir:/var/data` |
| :--- | :--- |




　　执行kubectl create命令创建该ConfigMap

| 12 | `#kubectl create -f cm-appvars.yamlconfigmap "cm-appvars.yaml"created` |
| :--- | :--- |


　　查看建立好的ConfigMap：

| 1234567891011121314151617181920212223242526 | `#kubectl get configmapNAME DATA AGEcm-appvars 2 3s[root@kubernetes-master ~]# kubectl describe configmap cm-appvarsName: cm-appvarsNamespace: defaultLabels: <none>Annotations: <none>Data====appdatadir: 9 bytesapploglevel: 4 bytes[root@kubernetes-master ~]# kubectl get configmap cm-appvars -o yamlapiVersion: v1data:appdatadir: /var/dataapploglevel: infokind: ConfigMapmetadata:creationTimestamp: 2017-04-14T06:03:36Zname: cm-appvarsnamespace: defaultresourceVersion:"571221"selfLink: /api/v1/namespaces/default/configmaps/cm-appvarsuid: 190323cb-20d8-11e7-94ec-000c29ac8d83　` |
| :--- | :--- |


　　另：创建一个cm-appconfigfile.yaml描述将两个配置文件server.xml和logging.properties定义为configmap的用法，设置key为配置文件的别名，value则是配置文件的文本内容：

| 1234567891011121314 | `apiVersion: v1kind: ConfigMapmetadata:name: cm-appvarsdata:key-serverxml:<?xml Version='1.0'encoding='utf-8'?><Server port="8005"shutdown="SHUTDOWN">.....</service></Server>key-loggingproperties:"handlers=lcatalina.org.apache.juli.FileHandler,...."` |
| :--- | :--- |


　　在pod "cm-test-app"定义中，将configmap "cm-appconfigfile"中的内容以文件形式mount到容器内部configfiles目录中。

Pod配置文件cm-test-app.yaml内容如下：

| 123456789101112131415161718192021 | `#vim cm-test-app.yamlapiVersion: v1kind: Podmetadata:name: cm-test-appspec:containers:- name: cm-test-appimage: tomcat-app:v1ports:- containerPort: 8080volumeMounts:- name: serverxml                          #引用volume名mountPath:/configfiles#挂载到容器内部目录configMap:name: cm-test-appconfigfile                  #使用configmap定义的的cm-appconfigfileitems:- key: key-serverxml                     #将key=key-serverxmlpath: server.xml                           #value将server.xml文件名进行挂载- key: key-loggingproperties                 #将key=key-loggingproperties    path: logging.properties                   #value将logging.properties文件名进行挂载　` |
| :--- | :--- |


　　创建该Pod：

| 12 | `#kubectl create -f cm-test-app.yamlPod "cm-test-app"created　　` |
| :--- | :--- |


　　登录容器查看configfiles目录下的server.xml和logging.properties文件，他们的内容就是configmap “cm-appconfigfile”中定义的两个key的内容

| 123 | `#kubectl exec -ti cm-test-app -- bashroot@cm-rest-app:/# cat /configfiles/server.xmlroot@cm-rest-app:/# cat /configfiles/logging.properties` |
| :--- | :--- |




　　5.3使用ConfigMap的条件限制

　　使用configmap的限制条件如下：

* * configmap必须在pod之间创建
  * configmap也可以定义为属于某个Namespace，只有处于相同namespaces中的pod可以引用
  * configmap中配额管理还未能实现
  * kubelet只支持被api server管理的pod使用configmap，静态pod无法引用
  * 在pod对configmap进行挂载操作时，容器内部职能挂载为目录，无法挂载文件。



