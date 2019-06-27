1、Docker进程通过监听宿主机的某个端口，将该端口的数据包发送给Docker容器

2、宿主机可以打开防火墙让局域网其他设备通过访问宿主机的端口进而访问docker的端口

这里以CDNS为例，CDNS是一个用于避免DNS污染的程序，通过CDNS可以把你的计算机变成一个抗污染的DNS服务器提供给局域网使用。Docker镜像下载地址：[https://hub.docker.com/r/alexzhuo/cdns/](https://hub.docker.com/r/alexzhuo/cdns/)

原理是在Docker容器中启动CDNS，监听53端口，Docker容器的IP地址为172.12.0.2，宿主机把5053端口映射到Docker容器上，访问宿主机的127.0.0.1:5053就相当于访问Docker的53端口，所以Docker的启动方法是：

sudo docker run -itd -p 0.0.0.0:5053:53/udp --name=CureDNS alexzhuo/cdns cdns -c /etc/cdns.config.json

这样我们使用dig工具通过5053端口查询DNS就是无污染的DNS了，过程如下：

$ dig www.facebook.com @127.0.0.1 -p 5053

