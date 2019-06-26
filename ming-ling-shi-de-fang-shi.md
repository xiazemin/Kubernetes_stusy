当使用命令式的命令时，用户直接对集群中的活动对象进行操作。用户提供kubectl命令的参数或标记进行操作。例子通过创建 Deployment 对象来运行 nginx 容器的实例:kubectl run nginx --image nginx使用不同的语法做同样的事情:kubectl create deployment nginx --image nginx命令式对象配置

在命令式对象配置中，kubectl命令指定操作\(创建，替换等\)，可选标志和至少一个文件名称。指定的文件必须包含对象的完整定义以 YAML 或 JSON 格式例子创建对象定义配置文件:kubectl create -f nginx.yaml删除两个配置文件中定义的对象:kubectl delete -f nginx.yaml -f redis.yaml通过覆写实时配置更新配置文件中定义的对象:kubectl replace -f nginx.yaml

