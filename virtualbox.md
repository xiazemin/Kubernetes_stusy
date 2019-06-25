VBoxManage是VirtualBox的命令行接口。利用他，你可以在主机操作系统的命令行中完全地控制VirtualBox。VBoxManage支持GUI可访问的全部功能，而且更多。VBoxManage展示了虚拟化引擎的全部特征，包括GUI无法访问的。

因为有了 Docker 这个东西。它依赖于 LXC\(Linux Container\)，能从网络上获得配置好的 Linux 镜像，非常容易在隔离的系统中运行自己的应用。也因为它的底层核心是个 LXC，所以在 Mac OS X 下需要在 VirtualBox 中跑一个精小的 LXC\(这里是一个 Tiny Core Linux，完全在内存中运行，个头只约 24MB，启动时间小于 5 秒的 boot2docker\) 虚拟机，构建在 VirtualBox 中。以后的通信过程就是 docker\(客户端\) --&gt; boot2docker --&gt; container，端口或磁盘映射也是遵照这一关系。



