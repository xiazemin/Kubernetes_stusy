为了使kubectl找到并访问Kubernetes集群，需要一个kubeconfig文件，当你使用kube-up.sh创建集群或成功部署Minikube集群时，该文件将自动创建。有关创建集群的更多信息，请参阅入门指南。如果你需要访问未创建的群集，请参阅共享群集访问文档。默认情况下，kubectl配置位于~/.kube/config。

检查kubectl配置

通过获取集群状态来检查kubectl是否正确配置：

$ kubectl cluster-info

如果看到一个URL响应，kubectl被正确配置为访问您的集群。

如果看到类似于以下内容的消息，则kubectl未正确配置：

The connection to the server &lt;server-name:port&gt; was refused - did you specify the right host or port?

启用shell自动完成

kubectl包括支持自动完成，可以节省大量打字！

完成脚本本身是由kubectl生成的，所以你通常只需要从你的配置文件中调用它。

这里提供常见的例子。有关详细信息，请咨询kubectl completion -h。

在Linux上，使用bash

要将kubectl自动完成添加到当前shell，请运行source &lt;\(kubectl completion bash\)。

要将kubectl自动完成添加到你的配置文件中，因此将在以后的shell中自动加载运行：

echo "source &lt;\(kubectl completion bash\)" &gt;&gt; ~/.bashrc

在MacOS上，使用bash

在macOS上，你需要首先通过Homebrew安装bash-completion支持：

\#\# If running Bash 3.2 included with macOS

brew install bash-completion

\#\# or, if running Bash 4.1+

brew install bash-completion@2

按照brew输出的“部分注意事项”，将正确的bash完成路径添加到本地的.bashrc中。

如果你使用Homebrew指令安装了kubectl，那么kubectl完成应该立即开始工作。

如果你手动安装了kubectl，则需要将kubectl自动完成添加到bash-completion中：

kubectl completion bash &gt; $\(brew --prefix\)/etc/bash\_completion.d/kubectl

Homebrew项目独立于kubernetes，所以bash完成包不能保证工作。

```
$    kubectl cluster-info
Kubernetes master is running at https://localhost:6443
KubeDNS is running at https://localhost:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```



