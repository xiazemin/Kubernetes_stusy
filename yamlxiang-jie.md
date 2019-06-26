YAML是专门用来写配置文件的语言，非常简洁和强大，使用比json更方便。它实质上是一种通用的数据串行化格式。后文会说明定义YAML文件创建Pod和创建Deployment。

YAML语法规则：

大小写敏感

使用缩进表示层级关系

缩进时不允许使用Tal键，只允许使用空格

缩进的空格数目不重要，只要相同层级的元素左侧对齐即可

”\#” 表示注释，从这个字符一直到行尾，都会被解析器忽略

详解

apiVersion: apps/v1 ：指定api版本，此值必须在kubectl apiversion中 ；查看版本 kubectl api-versions；v1是稳定版，v1beta1就是测试版

kind: Deployment：指在apps/v1 这个接口中指定资源的类型；Deployment是控制器

metadata: 指的资源Deployment（控制器）的元数据/属性

name: nginx-deployment 指定控制器名称

namespace: default 指定命名空间

labels: 指定标签

app: nginx

spec 指定这个资源（Deployment）内容

replicas: 3 设置3个副本

template: 以下就是容器的设置

metadata: 关联到上面的标签

labels:

app: nginx

spec:

containers:

name: nginx 容器的名称

image: nginx:1.15 镜像

ports:

containerPort: 80 容器的端口

执行：kubectl apply -f deployment.yaml



部署service，将应用发布出去

apiVersion: v1

kind: Service

metadata:

name: nginx-service

labels:

app: nginx

spec:

type: NodePort

ports:



port: 80

targetPort: 80

selector:

app: nginx

