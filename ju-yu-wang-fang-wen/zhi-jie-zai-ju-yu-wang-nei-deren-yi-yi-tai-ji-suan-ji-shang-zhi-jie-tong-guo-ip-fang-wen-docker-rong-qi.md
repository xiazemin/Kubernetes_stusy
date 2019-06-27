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



