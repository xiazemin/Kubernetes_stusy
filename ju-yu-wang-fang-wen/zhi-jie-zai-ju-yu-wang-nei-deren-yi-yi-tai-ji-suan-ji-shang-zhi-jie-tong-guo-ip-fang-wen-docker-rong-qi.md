首先通过观察发现，Docker的默认启动方式中，会产生一块虚拟网卡，在这里我们可以理解这块网卡连接着一个虚拟交换机，然后每个Docker容器又会拥有自己单独的网卡和IP，而且所有Docker容器也连接在这个虚拟交换机的下面。我们可以在宿主机上通过ifconfig命令看到这个虚拟网卡。

alex@alex-Lenovo-U310:~$ ifconfig

docker0   Link encap:以太网  硬件地址 02:42:cd:21:5c:81

```
      inet 地址:172.17.0.1  广播:0.0.0.0  掩码:255.255.0.0

      inet6 地址: fe80::42:cdff:fe21:5c81/64 Scope:Link

      UP BROADCAST RUNNING MULTICAST  MTU:1500  跃点数:1

      接收数据包:2892 错误:0 丢弃:0 过载:0 帧数:0

      发送数据包:3517 错误:0 丢弃:0 过载:0 载波:0

      碰撞:0 发送队列长度:0 

      接收字节:187022 \(187.0 KB\)  发送字节:4771886 \(4.7 MB\)
```

```
上面的docker0这块网卡就是虚拟网卡，它的IP地址是172.17.0.1，它和其他的docker容器都连接在一个虚拟交换机上，网段为172.17.0.0/16，下面我们登录到Docker容器里面，查看一下容器的网卡和IP
# ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:ac:11:00:02  
          inet addr:172.17.0.2  Bcast:0.0.0.0  Mask:255.255.0.0
          inet6 addr: fe80::42:acff:fe11:2/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:3449 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2811 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:4763490 (4.7 MB)  TX bytes:219998 (219.9 KB)
```

```
可以看到这个容器的IP地址为172.17.0.2，现在我们到宿主机里看看ping 172.17.0.2能不能ping通。
答案当然是能ping通，能ping通的原因就是我们的宿主机里知道目标地址为172.17.0.1/16的路由信息，不信我们可以查看一下

alex@alex-Lenovo-U310:~$ ip route
default via 192.168.12.1 dev wlp3s0  proto static  metric 600 
169.254.0.0/16 dev docker0  scope link  metric 1000 
172.17.0.0/16 dev docker0  proto kernel  scope link  src 172.17.0.1 
192.168.12.0/24 dev wlp3s0  proto kernel  scope link  src 192.168.12.107  metric 600
```

172.17.0.0/16这个网段的数据包可以通过docker0这块网卡发送出去。也就是说，目前宿主机有两个IP，一个是192.168.12.107,用于连接实体的局域网，一个是172.17.0.1，用来和Docker容器通信，从这可以看出宿主机和路由器的作用是一致的。而Docker容器只有一个IP就是172.17.0.2。如果docker容器想要访问外网，那么它就会把数据包发送到网关172.17.0.1，然后由宿主机做NAT发送到192.168.12.1/24这个网段的网关上。

不光宿主机可以ping通容器，而且由于在docker容器中默认路由（网关）是172.17.0.1,所以docker容器不光可以ping主机的172.17.0.1,还能ping通主机的另一个IP：192.168.12.107

此时我们的网络拓扑其实就变成了192.168.12.0/24这个网段里有个宿主机，为了方便理解，我们把这个宿主机看成一个路由器，路由器下面是172.17.0.1/16这个网段。我们把Docker容器看成实实在在的机器设备，连接在宿主机这个路由器的下面。这样Docker的拓扑结构就非常清晰了。我们可以发现这个拓扑结构其实非常的简单。就像家里上网的路由器一样。打个比方：我家里有两个路由器，一个路由器通过PPPOE拨号连接公网，内网地址为192.168.12.1，另一个路由器连接在第一个路由器上面，WAN口IP是192.168.12.107，LAN口地址是172.17.0.1，我们的Docker容器看成一个个的电脑接在第二个路由器LAN上面，所以Docker容器的IP为172.17.0.2。

第二个路由器（宿主机）通过NAT让我们的电脑们（Docker容器）可以访问互联网。电脑们（Docker容器们）可以互相ping通，也能ping通全部两个路由器。第二个路由器可以ping通电脑们，但是第一个路由器ping不同电脑们。

如果有一个电脑连接在第一个路由器的下面，和第二个路由器（宿主机）平级，其IP为192.168.12.113，现在它想ping通172.17.0.2这个Docker容器，发现是ping不通的。同样第一台路由器自己也是ping不通Docker容器的

原因很简单，这台新计算机只能ping通同网段的设备，比如路由器2，因为他们同属于192.168.12.1/24这个网段。而172.17.0.2/16这个网段它并不知道怎么路由过去，只能把目标地址为172.17.0.1/16的数据包发给路由器一，可惜就连第一个路由器也不知道怎么个路由法。所以我们就ping不通了。

所以现在问题就很好解决了，我们只需要告诉这台新电脑或者第一个路由器到172.17.0.2/16这个网段的路径就可以了。

于是我们可以在新电脑或者路由器一中这样写

route add -net 172.17.0.1/16 gw 192.168.12.107

或者是

ip route add 172.17.0.1/16 via 192.168.12.107

```
现在新电脑访问172.17.0.2的数据包就会先被发送到宿主机（第二个路由器）上，然后宿主机再转发到Docker容器上，我们就把Docker容器暴露到局域网里了。

但此时你会发现你在新计算机上还是ping不通，这是为什么呢。因为路由器二（宿主机）对它的内网机器也就是Docker容器们全部开启了NAT，源IP为172.17.0.2/16的数据包不会出现在宿主机以外的网络中，因为他们被NAT了。这个NAT是Docker进程默认自动帮我们实现的，我们先看一下

alex@alex-Lenovo-U310:~$ sudo iptables -t nat -L -n
Chain PREROUTING (policy ACCEPT)
target     prot opt source               destination         
DOCKER     all  --  0.0.0.0/0            0.0.0.0/0            ADDRTYPE match dst-type LOCAL

Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         
DOCKER     all  --  0.0.0.0/0           !127.0.0.0/8          ADDRTYPE match dst-type LOCAL

Chain POSTROUTING (policy ACCEPT)
target     prot opt source               destination         
MASQUERADE  all  --  172.17.0.0/16        0.0.0.0/0           
MASQUERADE  tcp  --  172.17.0.2           172.17.0.2           tcp dpt:53
MASQUERADE  udp  --  172.17.0.2           172.17.0.2           udp dpt:53

Chain DOCKER (2 references)
target     prot opt source               destination         
RETURN     all  --  0.0.0.0/0            0.0.0.0/0           
DNAT       tcp  --  0.0.0.0/0            127.0.0.1            tcp dpt:5053 to:172.17.0.2:53
DNAT       udp  --  0.0.0.0/0            127.0.0.1            udp dpt:5053 to:172.17.0.2:53

注意那句MASQUERADE  all  --  172.17.0.0/16        0.0.0.0/0会导致所有172.17.0.0/16的数据包都不能到达docker以外的网络，所以我们要关掉这个NAT，关掉很容易，我们只需删掉这一条iptables规则就可以了。然后源IP为172.17.0.2的数据包就可以出现在192.168.12.1/24的网络中了。
sudo iptables -t nat -D POSTROUTING 3
```



