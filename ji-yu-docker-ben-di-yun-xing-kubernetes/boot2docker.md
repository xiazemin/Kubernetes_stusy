安装时还是用的 boot2docker, 如今进化到了在 Mac OS X 下用 Docker Toolbox, 而且命令也由 boot2docker 换成了 docker-machine. 当然由于是非 Linux 系统, 所以 Mac OS X 仍然需要借助于 VirtualBox 中的 Linux 虚拟机作为桥梁, Docker Toolbox 创建的虚拟机名是 default \(boot2docker 创建的虚拟机名是 boot2docker-vm\) 就是这一桥梁, 我们称之为 DOCKER\_HOST. 文中的 default 虚拟机指的就是这个 DOCKER\_HOST.

```
$docker-machine ssh -h
Usage: docker-machine ssh [arg...]

Log into or run a command on a machine with SSH.

Description:
   Arguments are [machine-name] [command]
```


