Volume类型包括：emtyDir、hostPath、gcePersistentDisk、awsElasticBlockStore、gitRepo、secret、nfs、scsi、glusterfs、persistentVolumeClaim、rbd、flexVolume、cinder、cephfs、flocker、downwardAPI、fc、azureFile、configMap、vsphereVolume等等，可以定义多个Volume，每个Volume的name保持唯一。在同一个pod中的多个容器能够共享pod级别的存储卷Volume。Volume可以定义为各种类型，多个容器各自进行挂载操作，讲一个Volume挂载为容器内需要的目录。

如下图：

![](http://images2015.cnblogs.com/blog/1087716/201704/1087716-20170414113959283-1890538499.png)



　　如上图中的Pod中包含两个容器：tomcat和busybox，在pod级别设置Volume “app-logs”，用于tomcat想其中写日志文件，busybox读日志文件。

配置文件如下：

| 123456789101112131415161718192021 | `apiVersion:v1kind: Podmetadata:name: redis-phplabel:name: volume-podspec:containers:- name: tomcatimage: tomcatports:- containersPort: 8080volumeMounts:- name: app-logsmountPath:/usr/local/tomcat/logs- name: busyboximage:busyboxcommand: ["sh","-C","tail -f /logs/catalina*.log"]volumes:- name: app-logsemptyDir:{}` |
| :--- | :--- |


busybox容器可以通过kubectl logs查看输出内容

| 1 | `#kubectl logs volume-pod -c busybox　` |
| :--- | :--- |


tomcat容器生成的日志文件可以登录容器查看

| 1 | `#kubectl exec -ti volume-pod -c tomcat -- ls /usr/local/tomcat/logs` |
| :--- | :--- |


  


