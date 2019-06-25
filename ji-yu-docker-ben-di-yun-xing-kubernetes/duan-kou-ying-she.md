比如我们现在要做的映射关系是 Mac OS X\(50080\) --&gt; boot2docker\(40080\) --&gt; container\(80\)：

可以有两种办法



1\)boot2docker ssh -L 50080:localhost:40080  \#这条命令可以在  boot2docker-vm  运行时执行，建立多个不同的映射就是执行多次



docker run -i -t -p 40080:80 learn/tutorial

root@c79b5070a972:/\# apachectl start



然后在 Mac 的浏览器中打开 http://localhost:50080



2\)VBoxManage modifyvm "boot2docker-vm" --natpf1 "tcp-port\_50080:80,tcp,,50080,,40080"



docker run -i -t -p 40080:80 learn/tutorial

root@c79b5070a972:/\# apachectl start



这是直接修改了  boot2docker-vm 的配置，可以在 VirtualBox 中看到这条配置，配置 nat 命令见 http://www.virtualbox.org/manual/ch06.html\#natforward. 也能建立许多的端口映射



在 Mac 下使用 Docker 除了可用 boot2docker 作为 LXC，还有个替代品 VAGRANT 。

