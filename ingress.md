在本篇文章中你将会看到一些在其他地方被交叉使用的术语，为了防止产生歧义，我们首先来澄清下。



节点：Kubernetes集群中的服务器；

集群：Kubernetes管理的一组服务器集合；

边界路由器：为局域网和Internet路由数据包的路由器，执行防火墙保护局域网络；

集群网络：遵循Kubernetes网络模型实现群集内的通信的具体实现，比如flannel 和 OVS。

服务：使用标签选择器标识一组pod成为的Kubernetes Service。 除非另有说明，否则服务的虚拟IP仅可在集群内部访问。

什么是Ingress？

通常情况下，service和pod的IP仅可在集群内部访问。集群外部的请求需要通过负载均衡转发到service在Node上暴露的NodePort上，然后再由kube-proxy将其转发给相关的Pod。



而Ingress就是为进入集群的请求提供路由规则的集合，如下图所示



    internet

        \|

   \[ Ingress \]

   --\|-----\|--

   \[ Services \]

Ingress可以给service提供集群外部访问的URL、负载均衡、SSL终止、HTTP路由等。为了配置这些Ingress规则，集群管理员需要部署一个Ingress controller，它监听Ingress和service的变化，并根据规则配置负载均衡并提供访问入口。



Ingress格式

apiVersion: extensions/v1beta1

kind: Ingress

metadata:

  name: test-ingress

spec:

  rules:

  - http:

      paths:

      - path: /testpath

        backend:

          serviceName: test

          servicePort: 80

每个Ingress都需要配置rules，目前Kubernetes仅支持http规则。上面的示例表示请求/testpath时转发到服务test的80端口。



根据Ingress Spec配置的不同，Ingress可以分为以下几种类型：



单服务Ingress

单服务Ingress即该Ingress仅指定一个没有任何规则的后端服务。



apiVersion: extensions/v1beta1

kind: Ingress

metadata:

  name: test-ingress

spec:

  backend:

    serviceName: testsvc

    servicePort: 80

注：单个服务还可以通过设置Service.Type=NodePort或者Service.Type=LoadBalancer来对外暴露。



路由到多服务的Ingress

路由到多服务的Ingress即根据请求路径的不同转发到不同的后端服务上，比如



foo.bar.com -&gt; 178.91.123.132 -&gt; / foo    s1:80

                                 / bar    s2:80

可以通过下面的Ingress来定义：



apiVersion: extensions/v1beta1

kind: Ingress

metadata:

  name: test

spec:

  rules:

  - host: foo.bar.com

    http:

      paths:

      - path: /foo

        backend:

          serviceName: s1

          servicePort: 80

      - path: /bar

        backend:

          serviceName: s2

          servicePort: 80

使用kubectl create -f创建完ingress后：



$ kubectl get ing

NAME      RULE          BACKEND   ADDRESS

test      -

          foo.bar.com

          /foo          s1:80

          /bar          s2:80

虚拟主机Ingress

虚拟主机Ingress即根据名字的不同转发到不同的后端服务上，而他们共用同一个的IP地址，如下所示



foo.bar.com --\|                 \|-&gt; foo.bar.com s1:80

              \| 178.91.123.132  \|

bar.foo.com --\|                 \|-&gt; bar.foo.com s2:80

下面是一个基于Host header路由请求的Ingress：



apiVersion: extensions/v1beta1

kind: Ingress

metadata:

  name: test

spec:

  rules:

  - host: foo.bar.com

    http:

      paths:

      - backend:

          serviceName: s1

          servicePort: 80

  - host: bar.foo.com

    http:

      paths:

      - backend:

          serviceName: s2

          servicePort: 80

注：没有定义规则的后端服务称为默认后端服务，可以用来方便的处理404页面。



TLS Ingress

TLS Ingress通过Secret获取TLS私钥和证书\(名为tls.crt和tls.key\)，来执行TLS终止。如果Ingress中的TLS配置部分指定了不同的主机，则它们将根据通过SNI TLS扩展指定的主机名（假如Ingress controller支持SNI）在多个相同端口上进行复用。



定义一个包含tls.crt和tls.key的secret：



apiVersion: v1

data:

  tls.crt: base64 encoded cert

  tls.key: base64 encoded key

kind: Secret

metadata:

  name: testsecret

  namespace: default

type: Opaque

Ingress中引用secret：



apiVersion: extensions/v1beta1

kind: Ingress

metadata:

  name: no-rules-map

spec:

  tls:

    - secretName: testsecret

  backend:

    serviceName: s1

    servicePort: 80

注意，不同Ingress controller支持的TLS功能不尽相同。 请参阅有关nginx，GCE或任何其他Ingress controller的文档，以了解TLS的支持情况。



更新Ingress

可以通过kubectl edit ing name的方法来更新ingress：



$ kubectl get ing

NAME      RULE          BACKEND   ADDRESS

test      -                       178.91.123.132

          foo.bar.com

          /foo          s1:80

$ kubectl edit ing test

这会弹出一个包含已有IngressSpec yaml文件的编辑器，修改并保存就会将其更新到kubernetes API server，进而触发Ingress Controller重新配置负载均衡：



spec:

  rules:

  - host: foo.bar.com

    http:

      paths:

      - backend:

          serviceName: s1

          servicePort: 80

        path: /foo

  - host: bar.baz.com

    http:

      paths:

      - backend:

          serviceName: s2

          servicePort: 80

        path: /foo

..

更新后：



$ kubectl get ing

NAME      RULE          BACKEND   ADDRESS

test      -                       178.91.123.132

          foo.bar.com

          /foo          s1:80

          bar.baz.com

          /foo          s2:80

当然，也可以通过kubectl replace -f new-ingress.yaml命令来更新，其中new-ingress.yaml是修改过的Ingress yaml。



Ingress Controller

traefik ingress提供了一个traefik ingress的实践案例

kubernetes/ingress提供了更多的Ingress示例

参考文档

Kubernetes Ingress Resource

Kubernetes Ingress Controller

使用NGINX Plus负载均衡Kubernetes服务

使用 NGINX 和 NGINX Plus 的 Ingress Controller 进行 Kubernetes 的负载均衡

Kubernetes : Ingress Controller with Træfɪk and Let’s Encrypt

Kubernetes : Træfɪk and Let’s Encrypt at scale

Kubernetes Ingress Controller-Træfɪk

Kubernetes 1.2 and simplifying advanced networking with Ingress

https://feisky.gitbooks.io/kubernetes/concepts/ingress.html

