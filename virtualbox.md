VBoxManage是VirtualBox的命令行接口。利用他，你可以在主机操作系统的命令行中完全地控制VirtualBox。VBoxManage支持GUI可访问的全部功能，而且更多。VBoxManage展示了虚拟化引擎的全部特征，包括GUI无法访问的。

因为有了 Docker 这个东西。它依赖于 LXC\(Linux Container\)，能从网络上获得配置好的 Linux 镜像，非常容易在隔离的系统中运行自己的应用。也因为它的底层核心是个 LXC，所以在 Mac OS X 下需要在 VirtualBox 中跑一个精小的 LXC\(这里是一个 Tiny Core Linux，完全在内存中运行，个头只约 24MB，启动时间小于 5 秒的 boot2docker\) 虚拟机，构建在 VirtualBox 中。以后的通信过程就是 docker --&gt; boot2docker --&gt; container，端口或磁盘映射也是遵照这一关系。

Docker 安装过程

1. 安装 [VirtualBox](javascript:void%28%29), 不多讲, 因要在它当中创建一个 boot2docker-vm 虚拟机

2. 安装 boot2docker

brew install boot2docker

你也可以手工安装

curl https://raw.github.com/steeve/boot2docker/master/boot2docker&gt; boot2docker; chmod +x boot2docker; sudo mv boot2docker /usr/local/bin

3. 安装 Docker

brew install docker

也可手工安装

curl -o docker http://get.docker.io/builds/Darwin/x86\_64/docker-latest; chmod +x docker; sudo cp docker /usr/local/bin

4. 配置 Docker 客户端

export DOCKER\_HOST=tcp://127.0.0.1:4243

把它写到 ~/.bash\_profile 中，如果你是用的 bash 的话。我工作在 fish 下，所以在 ~/.config/fish/config.fish 中加了 set -x DOCKER\_HOST tcp://127.0.0.1:4243

5. boot2docker 初始化与启动

boot2docker init

完成后就能在 VirtualBox 中看到一个叫做 boot2docker-vm的虚拟机，以后只需用 boot2docker 命令来控制这个虚拟机的行为，启动，停止等。

boot2docker up

启动，boot2docker-vm虚拟机，我们能在 VirtualBox 中看到该虚拟机变成 Running 状态

直接执行 boot2docker 可以看到可用的参数

Usage /usr/local/bin/boot2docker {init\|start\|up\|save\|pause\|stop\|restart\|status\|info\|delete\|ssh\|download}

6. 启动 Docker 守护进程

sudo docker -d

这时可执行

boot2docker ssh，输入密码  tcuser 进到该虚拟机的控制台下，如果要用户名的话请输入docker

