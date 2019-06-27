

如何让容器中的主机与宿主机360 ° 无死角通信

最近工作环境换成了Mac 体验还不错，在高负载的情况下表现也十分出色，然后Mac平台我不想装太多渗透软件，装好保持较干净的环境。但是我又不想每天都开着 vm or vbox 虚拟机，太不方便。所以就想要不直接装个docker，然后pull一个kali的镜像，这样每次使用的时候就会极为方便，再加上X11的存在，我也不怕GUI出不来。但是在做准备这个渗透环境的时候除了不少问题，以下总结一下。







我们知道官方并没有一种容器和宿主机实现桥接的解决方案，但是我们可以通过自建虚拟网卡的方式来做



下面是官方的说辞:



There is no docker0 bridge on macOS



Because of the way networking is implemented in Docker for Mac, you cannot see a docker0 interface on the host. This interface is actually within the virtual machine.







I cannot ping my containers



Docker for Mac can’t route traffic to containers.







Per-container IP addressing is not possible



The docker \(Linux\) bridge network is not reachable from the macOS host.







Use cases and workarounds



There are two scenarios that the above limitations affect:



I WANT TO CONNECT FROM A CONTAINER TO A SERVICE ON THE HOST



The host has a changing IP address \(or none if you have no network access\). From 18.03 onwards our recommendation is to connect to the special DNS name host.docker.internal, which resolves to the internal IP address used by the host.



The gateway is also reachable as gateway.docker.internal.







I WANT TO CONNECT TO A CONTAINER FROM THE MAC



Port forwarding works for localhost; --publish, -p, or -P all work. Ports exposed from Linux are forwarded to the host.



Our current recommendation is to publish a port, or to connect from another container. This is what you need to do even on Linux if the container is on an overlay network, not a bridge network, as these are not routed.



The command to run the nginx webserver shown in Getting Started is an example of this.



1

$ docker run -d -p 80:80 --name webserver nginx

……



主要说了这么几点：



Mac os 上是没有docker0 这块儿网卡的

你无法发送ping 到你的容器中

默认的桥接是实现不了容器与主机处于同网段

你想访问容器需要通过端口映射的方式来做

但是秉着不放弃精神我尝试了很多方法，踩了很多坑这里就不一一说了，最后在Github上找到了两个比较好的解决方案。



借助TUNTAP虚拟网卡

https://github.com/mal/docker-for-mac-host-bridge



先放上链接



作者给出了这几步来完成：



Download the tuntap OSX kernel extensions

Extract the .pkg file within the tuntap archive

Download install.sh

\(Optional, but encouraged\) Read install.sh!

Run install.sh \(see example below\)

1

DOCKER\_TAP\_NETWORK=acme ./install.sh tuntap\_20150118.pkg

我在做完这步后并没有出现tuntap网卡 然后通过重启搞出来了



然后docker 网络中你会发现你多了一块名为acme的虚拟网卡



1

2

3

4

5

6

➜  docker-for-mac-host-bridge git:\(master\) ✗ docker network ls                            

NETWORK ID          NAME                DRIVER              SCOPE

e166944bf57f        acme                bridge              local

d42caf83713f        bridge              bridge              local

cd67fa698374        host                host                local

1c25d51a2292        none                null                local

这个时候就试检验的时候了



1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

36

➜  docker-for-mac-host-bridge git:\(master\) ✗ docker run --rm -it --net=acme brimstone/kali

root@5d5a6712feda:/pentest\# ifconfig 

eth0: flags=4163&lt;UP,BROADCAST,RUNNING,MULTICAST&gt;  mtu 1500

        inet 172.18.0.2  netmask 255.255.0.0  broadcast 172.18.255.255

        ether 02:42:ac:12:00:02  txqueuelen 0  \(Ethernet\)

        RX packets 3  bytes 258 \(258.0 B\)

        RX errors 0  dropped 0  overruns 0  frame 0

        TX packets 0  bytes 0 \(0.0 B\)

        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0



lo: flags=73&lt;UP,LOOPBACK,RUNNING&gt;  mtu 65536

        inet 127.0.0.1  netmask 255.0.0.0

        loop  txqueuelen 1  \(Local Loopback\)

        RX packets 0  bytes 0 \(0.0 B\)

        RX errors 0  dropped 0  overruns 0  frame 0

        TX packets 0  bytes 0 \(0.0 B\)

        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0



root@5d5a6712feda:/pentest\# ping bing.com

PING bing.com \(204.79.197.200\) 56\(84\) bytes of data.

64 bytes from a-0001.a-msedge.net \(204.79.197.200\): icmp\_seq=1 ttl=37 time=61.0 ms

64 bytes from a-0001.a-msedge.net \(204.79.197.200\): icmp\_seq=2 ttl=37 time=64.2 ms

^C

--- bing.com ping statistics ---

2 packets transmitted, 2 received, 0% packet loss, time 1003ms

rtt min/avg/max/mdev = 61.096/62.659/64.222/1.563 ms

root@5d5a6712feda:/pentest\# exit

exit



➜  docker-for-mac-host-bridge git:\(master\) ✗ ifconfig tap1

tap1: flags=8943&lt;UP,BROADCAST,RUNNING,PROMISC,SIMPLEX,MULTICAST&gt; mtu 1500

	ether ca:26:73:fc:29:31 

	inet 172.18.0.1 netmask 0xffff0000 broadcast 172.18.255.255

	media: autoselect

	status: active

	open \(pid 7227\)

上网也能上 而且还是同网段 这下就安心了。

