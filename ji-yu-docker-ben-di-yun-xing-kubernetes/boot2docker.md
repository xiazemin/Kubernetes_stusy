安装时还是用的 boot2docker, 如今进化到了在 Mac OS X 下用 Docker Toolbox, 而且命令也由 boot2docker 换成了 docker-machine. 当然由于是非 Linux 系统, 所以 Mac OS X 仍然需要借助于 VirtualBox 中的 Linux 虚拟机作为桥梁, Docker Toolbox 创建的虚拟机名是 default \(boot2docker 创建的虚拟机名是 boot2docker-vm\) 就是这一桥梁, 我们称之为 DOCKER\_HOST. 文中的 default 虚拟机指的就是这个 DOCKER\_HOST.

[https://github.com/boot2docker/boot2docker\#ssh-into-vm](https://github.com/boot2docker/boot2docker#ssh-into-vm)

```
$docker-machine ssh -h
Usage: docker-machine ssh [arg...]

Log into or run a command on a machine with SSH.

Description:
   Arguments are [machine-name] [command]
```

```
~$              boot2docker ssh -L8080:localhost:8080
error in run: Failed to get machine "boot2docker-vm": exec: "VBoxManage": executable file not found in $PATH
```

下载

[https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

     boot2docker ssh -L8080:localhost:8080
    error in run: Failed to get machine "boot2docker-vm": machine does not exist (Did you run `boot2docker init`?)

    $boot2docker init

      WARNING: The 'boot2docker' command line interface (not to be confused with
      'boot2docker' the operating system) is officially deprecated.

      Please switch to Docker Machine (https://docs.docker.com/machine/) ASAP.

      Docker Toolbox (https://docker.com/toolbox) is the recommended install method.

    Initialization of virtual machine "boot2docker-vm" complete.
    Use `boot2docker up` to start it.

```
$boot2docker up

  WARNING: The 'boot2docker' command line interface (not to be confused with
  'boot2docker' the operating system) is officially deprecated.

  Please switch to Docker Machine (https://docs.docker.com/machine/) ASAP.

  Docker Toolbox (https://docker.com/toolbox) is the recommended install method.

Waiting for VM and Docker daemon to start...
................................................................... .......o o o o
```



