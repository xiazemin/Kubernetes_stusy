Replication Controller 保证了在所有时间内，都有特定数量的Pod副本正在运行，如果太多了，Replication Controller就杀死几个，如果太少了，Replication Controller会新建几个，和直接创建的pod不同的是，Replication Controller会替换掉那些删除的或者被终止的pod，不管删除的原因是什么（维护阿，更新啊，Replication Controller都不关心）。基于这个理由，我们建议即使是只创建一个pod，我们也要使用Replication Controller。Replication Controller 就像一个进程管理器，监管着不同node上的多个pod,而不是单单监控一个node上的pod,Replication Controller 会委派本地容器来启动一些节点上服务（Kubelet ,Docker）。



正如我们在pod的生命周期中讨论的，Replication Controller只会对那些RestartPolicy = Always的Pod的生效，（RestartPolicy的默认值就是Always）,Replication Controller 不会去管理那些有不同启动策略pod



如我们在issue \#503讨论的，我们希望在将来别的控制器被加入到Kubernete中来管理一些例如负载阿，测试等相关功能



Replication Controller永远不会自己关闭，但是，我们并不希望Replication Controller成为一个长久存在的服务。服务可能会有多个Pod组成，这些Pod又被多个Replication Controller控制着，我们希望Replication Controller 会在服务的生命周期中被删除和新建（例如在这些pod中发布一个更新），对于服务和用户来说，Replication Controller是通过一种无形的方式来维持着服务的状态



Replication Controller是如何工作的

Pod template



一个Replication Controller通过模版来创建pod,这个是Replication Controller对象内置的功能，但是我们计划要将这个功能从Replication Controller剥离开来



与其说Pod的模版是一个多有Pod副本的期望状态，Pod的模版更像是一个饼干的模具，一旦从模具中出来之后，饼干就和饼干模具美啥关系了，没有任何关联。模版的改变甚至是模版的更改对已经存在的pod没有任何影响，Replication Controller创建的pod可以在之后直接的修改，这对Pod来说是非常重要的，这样就定制了Pod中的所有容器的期望状态。这从根本上简化系统复杂度增加了灵活性，正如下面的这个例子



Replication Controller 创建的的Pod 是可以相互替代和语义上相同的，尽管他们的配置文件可能会随着是时间的变化而不一样，这非常适合无状态服务器，但是Replication Controller也可以被用在保持主从的高可用，共享，work-pool类应用程序，这样的程序应该使用的是动态分配机制，例如etcd lock module和RabbitMQ 的队列，相对于静态/一次性的定制的Pod的配置文件，应该是一种反模式，任何定制化的pod,例如垂直的自动变化资源（cpu,内存），应该被其它的线上Controller管理，而不是Replication Controller.



Labels

由Replication Controller监控的Pod的数量是是由一个叫 label selector（标签选择器）决定的，label selector在Replication Controller和被控制的pod创建了一个松散耦合的关系,与pod相比，pod与他们的定义文件关系紧密。我们故意的不使用固定长度的数组来存储被管理的pod,因为根据我们的经验，如果那样作了，会增加管理的复杂度，不论是系统还是客户



replication controller 需要确认那些从特定模版创建出来的pod拥有label selector所要求的标签，尽管，它还没有这么作，我们需要通过确认replication controllers的label selectors 没有覆盖设置来确定仅有一个Replication Controller控制所指定的Pod（这段话有点怪）



记住这个：replication controller自己也可以由标签，would generally carry the labels their corresponding pods have in common，但是这些标签并不影响replication controller的行为



Pod会被replication controller删除，如果修改那些pod的标签，这个功能可能会被应用在从服务中删除pod来作测试，数据恢复等。Pod如果是以这种方式被删除的话，那么那个pod会被自动的替换（前提是宗的副本数量未修改）



类似的，删除一个replication controller不会影响它创建的pod,如果向删除那些它那些控制的pod，需要首先拔replcas的值设置为0（注意，用户端工具kubectl 提供了一个命令stop,来删除replication controller以及它控制的pod,但是，这个功能在现在的api中不存在）



Responsibilities of the replication controller（replication controller的责任）



replication controller的任务就是保证预计数量的pod并并保证其可用性，目前，只有那些被终止的Pod是从总数量中排除的，在将来，可读性以及其它系统信息可能会被纳入统计，我们肯能会增加更多的替代策略，并且我们计划可以执行其它外部程序可以使使用并实现复杂的替换或者策略



replication controller的任务永远都只会是单一的。它自身不会进行是否可读和是否可用的检测，相比与自动进行缩放和放大，replication controller更倾向与使用外部的自动平衡工具，这些外部工具要作的仅仅是修改replicas的值来实现Pod数量的变化，我们不会增加replication controller的调度策率，也不会让replication controller来验证受控的Pod是否符合特定的模版，因为这些都会阻碍自动调整和其它的自动的进程。类似的，完成时限，需求依赖，配置扩展，等等都属于其它的部分。



replication controller的目的是成为一个原始的积木，我们希望在replication controller上层的api或者工具来为用户提供方便，现在kubectl支持的宏观操作比如run,stop,scale,rolling-update就是这个概念的例子，举例来说，我们可以想象和“天庭”管理着replication controller，auto-scalers, services, scheduling policies, canaries, 等等



常见的使用模式

Rescheduling（重新规划）

正如我们之前提到的，无论你是有1个或者有1000个pod需要运行，Replication Controller 会确保该数量的pod在运行，甚至在节点（node）失败或者节点（node）终止的情况下



Scaling（缩放）

Replication Controller让我们更容易的控制pod的副本的数量，不管我们是手动控制还是通过其它的自动管理的工具，最简单的：修改replicas的值



Rolling updates（动态更新）

Replication Controller 可以支持动态更新，当我们更新一个服务的时候，它可以允许我们一个一个的替换pod



正如我们在\#1353中解释的，推荐的方法是创建一个新的只有1个pod副本的Replication Controller,然后新的每次+1，旧的每次-1，直到旧的Replication Controller 的值变成0，然后我们就可以删除旧的了，这样就可以规避省级过程中出现的未知错误了



理论上，动态更新控制器应考虑应用的可用性，并确保足够的pod制成服务在任何时间都能正常提供服务



两个 Replication Controller创建的pod至少要由一个不同的标签，可以是镜像的标签，因为一般镜像的更新都会带来一个新的更新



kubectl是实现动态更新的客户端



Multiple release tracks多个发布版本追踪

除了在程序更新过程中同时运行多个版本的程序外，在更新完成之后的一段时间内或者持续的同时运行多个版本（新旧），通过多国版本的追踪（版本的追踪是通过label来实现的）



举个例子来说：一个服务可能绑定的Pod为tier in \(frontend\), environment in \(prod\)，现在我们旧假设我们由10个副本来组成这个tier,现在我们要发布一个新的版本canary，这个时候，我们就应该设置一个Replication Controller，并且设置replcas的值为9，并且标签为tier=frontend, environment=prod, track=stable，然后再设置另外一个Replication Controller，并且把replacas的值设置为1标签为：tier=frontend, environment=prod, track=canary.现在这个服务就同时使用了新版和旧版两个版本了，这个时候我们旧可以通过不同的Replication Controller进行一些测试，监控…

