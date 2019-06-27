Docker for Mac 自诞生以来就一直有一个问题，那就是在宿主机上看不到 docker0，无法访问容器所在的网络，也就是说不能 ping 通 Docker 给 Container 所分配的 IP 地址。关于这个问题，官方文档中有描述，Known limitations, use cases, and workarounds。对于 docker run 启动的 Container 来说，通常会通过 -p 参数映射相应的服务端口，一般不会遇到要直接访问容器 IP 的情况。但对于 Kubernetes 来讲，很多时候都想要直接访问 Pod IP 或者 Service IP 进行调试，在 Mac 上却没办法实现。



Docker for Mac 基本原理

要解决这个问题，得先搞清楚 Docker for Mac的原理。我们都知道 Docker 是利用 Linux 的 Namespace 和 Cgroups 来实现资源的隔离和限制，容器共享宿主机内核，所以 Mac 本身没法运行 Docker 容器。不过不支持不要紧，我们可以跑虚拟机，最早还没有 Docker for Mac 的时候，就是通过 docker-machine 在 Virtual Box 或者 VMWare 直接起一个 Linux 的虚拟机，然后在主机上用 Docker Client 操作虚拟机里的 Docker Server。

